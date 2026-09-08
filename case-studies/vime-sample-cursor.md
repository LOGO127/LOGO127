# A rollout data source should preserve its sample stream

September 8, 2026 · CPU correctness validation · **reported, patch unpublished**

Public reproducer: [Vime #414](https://github.com/vllm-project/vime/issues/414).
The issue includes a standalone script using the actual Dataset and
RolloutDataSource. No accepted fix or module ownership is claimed.

## The contract and the defect

A request for N prompt groups should agree with N successive single-group
requests: the same prompts, group IDs, sample IDs and next cursor position.
Epoch transitions must also apply the same shuffle sequence.

On main [`ce92eff`](https://github.com/vllm-project/vime/commit/ce92eff12ecdc81396bf41a2f94e62dd5b0aca32),
the source handles only one epoch wrap per call. A three-row dataset and
`get_samples(8)` yield:

| Observation | Original source | Local candidate |
| --- | --- | --- |
| Returned groups | 6 | 8 |
| Prompt order, shuffle disabled | 0, 1, 2, 0, 1, 2 | 0, 1, 2, 0, 1, 2, 0, 1 |
| Epoch / offset | 1 / 5 | 2 / 2 |

An offset of five is outside the three-row dataset. The original implementation
also saves this cursor state, so checking only the number returned on the first
call misses the continuation and resume boundary.

## Validation, including controls

The candidate traverses as many epoch boundaries as necessary, retaining the
existing lazy transition at an exact boundary and the existing sample-copy logic.
It explicitly rejects positive requests against an empty dataset rather than
introducing an infinite loop. The issue asks maintainers whether wrapping or
rejecting oversized requests is their preferred contract.

Tests exercise real local JSONL reading, sample construction, group/index
assignment and `torch.save`/`torch.load` of cursor state. Automatic model loading
is disabled and a real Dataset is supplied explicitly; tokenizer, processor and
length filtering are disabled. No implementation is replaced by an import stub.

- Counts 0, 1, 2, 3, 5, 6, 8 and 11; shuffle enabled and disabled.
- Repeated mixed-size requests with dataset sizes 1, 3 and 7.
- Buffered groups followed by multi-epoch top-up.
- Cursor save/resume and independent copies of repeated prompts.
- Fourteen existing adjacent rollout-data tests retained as controls.

The matched suite produced **16 failures / 26 passes on the baseline** and
**42 passes on the candidate**. One failure tests the newly proposed empty-data
error behavior; it is not evidence of a previously documented exception promise.
All nine configured pre-commit hooks passed on both versions in a native Linux
clone, using the project's pinned tools. Initial hook setup failed to locate Ruff;
an isolated runner environment corrected that setup and the checks were rerun
without skipping hooks.

## What this does not prove

The rollout scheduler can request more data after an underfilled response. These
tests therefore do not establish a final rollout batch-size failure, training
hang or model-quality regression. They establish the data-source return/order
and cursor contract, not every deployment consequence.

Environment: WSL Ubuntu, Python 3.12.3 and PyTorch 2.11.0+cpu. No live vLLM,
Megatron training, GPU/NPU, distributed rollout or throughput benchmark was run.
Investigation, tests and patch were AI-assisted. Human review, end-to-end
validation and DCO required by Vime remain gates before publishing a PR.
