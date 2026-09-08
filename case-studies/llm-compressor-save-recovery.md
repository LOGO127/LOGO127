# When a failed checkpoint save leaves ranks on different paths

September 8, 2026 · CPU correctness investigation · **reported, not fixed or merged**

Public report: [LLM Compressor #3149](https://github.com/vllm-project/llm-compressor/issues/3149).
The report includes a download-free single-process reproducer and the observed
two-rank failure. Maintainer scope/assignment is pending. This is not a claim of
module ownership or an accepted code contribution.

## The question

While investigating the project's proposed streaming checkpoint work, I checked
what happens when the existing save operation fails. Its wrapper temporarily
converts compressed-tensors (CT) offloading to Accelerate offloading, saves model
weights and metadata, then converts the live model back. The restoration call
was on the success path, outside exception-safe cleanup.

That is a lifecycle question rather than a model-quality benchmark: does the
live model return to a usable, consistent offload representation after failure,
and do distributed ranks still enter matching collectives?

## Test design

I instantiated a one-layer Llama from a tiny configuration, with random weights
and CPU offload caches. No model weights, datasets, GPU runtime or credentials
were needed. The real save wrapper received an existing file as its output
directory. Transformers logged the invalid destination; the subsequent recipe
write raised `NotADirectoryError`.

This is a real metadata-path failure. It is **not** a disk-full experiment or an
injected serializer exception.

| Observation | CT parameter caches | Accelerate hooks |
| --- | ---: | ---: |
| Before saving | 20 | 0 |
| After the metadata exception | 0 | 20 |
| After explicitly invoking recovery | 13 | 0 |

The fixture initially offloads empty modules too; recovery reconstructs dispatch
for parameter-bearing modules, so the two healthy cache counts need not match.
After explicit recovery, forward logits matched the original exactly. These
results do not demonstrate corrupted weights, nor do they prove inference must
fail in the temporary representation.

## Why two ranks matter

The recovery function calls `broadcast_object_list`. Under two CPU/Gloo workers,
the source rank raised the recipe error and skipped recovery. The other rank
entered recovery, removed its Accelerate hooks, and timed out waiting for the
source's broadcast. It ended with neither CT caches nor Accelerate hooks.

The default process group had a five-second timeout. A separate control group
kept both workers alive and collected their exception traces, so the peer error
was a collective timeout rather than an artifact of terminating rank 0 early.
No production functions were replaced.

For a matched positive control, I ran the same fixture with a valid output
directory. Both ranks saved/recovered successfully; both live models' logits
were unchanged. The source rank also reloaded the actual checkpoint and compared
its logits exactly. A fresh invalid-directory run reproduced the timeout again.

## Reproducibility and limits

- LLM Compressor source: [`8f96fe61`](https://github.com/vllm-project/llm-compressor/commit/8f96fe61feb501b98c2f1c61de2053b50f8d8534).
- Compressed Tensors source: [`099fa98f`](https://github.com/vllm-project/compressed-tensors/commit/099fa98fea7f3533a8e304a081795a1136ef67c0).
- WSL Linux, Python 3.12.3, Torch 2.11.0+cpu; source packages built into isolated
  install targets. Shallow build version strings were not treated as releases.
- The first run used Transformers 5.14.1. Dependency auditing caught that it was
  below the checkout's declared minimum. I reran with 5.15.0 and reproduced the
  same result; the distributed experiments used 5.15.0.
- No quantized-GPU, NCCL, actual storage-exhaustion or full-suite result is claimed.
  The success control verifies a small CPU model, not large-model memory bounds.
- Investigation and diagnostic code were AI-assisted. No fix has been submitted.

## What a repair must establish

A robust repair needs coordinated exceptional control flow, not just a passing
single-process cleanup test. It should restore state after successful temporary
conversion, preserve meaningful original errors, and prevent peers from entering
unmatched recovery collectives. Recovery itself can fail, so that policy also
needs review. These are requirements to resolve with maintainers, not claims
that the implementation is already complete.
