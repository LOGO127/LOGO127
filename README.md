<div align="center">

<img src="https://raw.githubusercontent.com/LOGO127/LOGO127/main/assets/header.svg" alt="LOGO127 — AI Systems, LLM Training, and Inference" width="100%" />

Master's student at Zhejiang University. I build small, reproducible systems
to understand language models and turn real engineering problems into clear,
reviewable open-source work.

[Projects](https://github.com/LOGO127?tab=repositories) · [Contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md)

</div>

**Merged upstream:** [vLLM-Omni #7065](https://github.com/vllm-project/vllm-omni/pull/7065) — fixed Higgs Audio v3 voice-clone token validation while preserving reference-audio placement across chunked prefill.

---

## What I work on

| Area | Focus |
| --- | --- |
| **LLM foundations** | Tokenization, Transformer components, optimization, training, and evaluation |
| **AI systems** | Local-first tools, inference workflows, reproducibility, and observability |
| **AI for Science** | Scientific reasoning and data-driven lithium-ion battery research |

## Featured work

| Project | What it shows |
| --- | --- |
| [wechat-ai-memory](https://github.com/LOGO127/wechat-ai-memory) | A local-first Windows application that turns conversations into traceable PDF, Markdown, and JSON context |
| [CS336 Assignment 1](https://github.com/LOGO127/cs336-2026-assignment1-llm-foundations) | From-scratch BPE, Transformer, AdamW, training, generation, tests, and measured TinyStories results |
| [cs336.2026](https://github.com/LOGO127/cs336.2026) | Systems-first notes and implementation records for Stanford CS336: Language Modeling from Scratch |

## Open-source work

I prefer the engineering loop:

**reproduce → understand the contract → make the smallest useful change → test → explain the evidence**

### Selected signals

| Area | Evidence | Current state |
| --- | --- | --- |
| **LLM serving** | [vLLM #55210](https://github.com/vllm-project/vllm/pull/55210) — streaming reasoning-parser boundaries | Human-approved; waiting for upstream CI |
| **Multimodal serving** | [vLLM-Omni #7065](https://github.com/vllm-project/vllm-omni/pull/7065) — Higgs Audio v3 voice-clone token validation | **Merged** after collaborator review; core CI passed |
| **Local AI runtime** | [Ollama #18234](https://github.com/ollama/ollama/pull/18234) — rootless Linux installation | Open; mergeable; review pending |
| **Graph ML** | [PyG #10797](https://github.com/pyg-team/pytorch_geometric/pull/10797) — ONNX-safe scatter min/max path | Open; CI passed; review pending |
| **Vector search** | [Faiss #5574](https://github.com/facebookresearch/faiss/pull/5574) — Canberra zero-denominator correctness | Open; Meta CLA passed; import CI/review pending |
| **Training systems** | [LLaMA-Factory #10813](https://github.com/hiyouga/LlamaFactory/pull/10813) — tied-weight serialization · [GPT-NeoX #1419](https://github.com/EleutherAI/gpt-neox/pull/1419) — clean-runner CI setup · [DeepSpeed #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411) — ZeRO gradient-norm fallback | Open; targeted checks passed; review pending |
| **RL training** | [PrimeRL #3489](https://github.com/PrimeIntellect-ai/prime-rl/pull/3489) — W&B GraphQL transport compatibility | Ready for review; old/new W&B checks passed; maintainer CI approval pending |
| **Training infrastructure** | [OLMo-core #858](https://github.com/allenai/OLMo-core/pull/858) — optional Beaker dependency isolation · [OLMo-core #860](https://github.com/allenai/OLMo-core/pull/860) — async-checkpoint host-memory release | Both open; focused regressions and static checks passed; review pending |
| **Model optimization** | [bitsandbytes #2073](https://github.com/bitsandbytes-foundation/bitsandbytes/pull/2073) — explicit ROCm architecture override for restricted containers | Open; 13 tests and pre-commit passed; upstream CI/review pending |
| **Runtime correctness** | [ONNX Runtime #32435](https://github.com/microsoft/onnxruntime/pull/32435) — identity transpose/Gemm fusion | CLA passed; native CPU regressions passed; upstream CI/re-review pending |
| **Kernel/compiler work** | [FlashAttention #2858](https://github.com/Dao-AILab/flash-attention/pull/2858) · [Triton #11580](https://github.com/triton-lang/triton/pull/11580) | Open; hardware CI/review pending |

<details>
<summary>Full upstream contribution log · 1 merged + 18 open PRs</summary>

- [PrimeRL #3489](https://github.com/PrimeIntellect-ai/prime-rl/pull/3489) — restores W&B dashboard startup across legacy and current SDKs by reusing the project's existing compatible GraphQL transport.
- [OLMo-core #860](https://github.com/allenai/OLMo-core/pull/860) — releases staged model and optimizer tensors before later async-checkpoint callbacks retain substantial host memory.
- [OLMo-core #858](https://github.com/allenai/OLMo-core/pull/858) — lets the default Beaker callback detect its runtime environment without importing optional Beaker/Gantry dependencies during attachment.
- [bitsandbytes #2073](https://github.com/bitsandbytes-foundation/bitsandbytes/pull/2073) — lets restricted ROCm containers provide a validated GPU architecture without invoking an unavailable `rocminfo` binary.
- [GPT-NeoX #1419](https://github.com/EleutherAI/gpt-neox/pull/1419) — installs the test job's dependencies before its dataset-preparation import path runs on a clean CI worker.
- **Merged:** [vLLM-Omni #7065](https://github.com/vllm-project/vllm-omni/pull/7065) — keeps Higgs Audio v3 voice-clone prompts vocabulary-valid while preserving reference-audio embedding positions across chunked prefill.
- [LLaMA-Factory #10813](https://github.com/hiyouga/LlamaFactory/pull/10813) — restores tied-parameter aliases lost while DeepSpeed gathers a full state dict, preventing redundant Qwen3 `lm_head` serialization.
- [Ollama #18234](https://github.com/ollama/ollama/pull/18234) — documents a user-local Linux installation under `~/.local`, including PATH and architecture/backend variants.
- [PyTorch Geometric #10797](https://github.com/pyg-team/pytorch_geometric/pull/10797) — avoids Tensor-valued fill arguments in the ONNX scatter path and adds regression coverage.
- [Faiss #5574](https://github.com/facebookresearch/faiss/pull/5574) — treats shared-zero Canberra terms as zero and protects `IndexFlat.search` from NaN results.
- [Hugging Face TRL #7038](https://github.com/huggingface/trl/pull/7038) — fail-fast guard for the incompatible MoE auxiliary-loss and Liger GRPO combination.
- [OpenAI Python #3790](https://github.com/openai/openai-python/pull/3790) — removes unreachable SSE error checks from sync and async streaming paths.
- [vLLM #55210](https://github.com/vllm-project/vllm/pull/55210) — fixes reasoning-parser boundaries across `<think>` / `</think>` markers.
- [Stanford CS336 lectures #47](https://github.com/stanford-cs336/lectures/pull/47) — clarifies the Lecture 11 optimal-batch caption.
- [SGLang #37935](https://github.com/sgl-project/sglang/pull/37935) — reports tensor-backed DeepSeek-V4 KV-cache memory across pooled layouts.
- [DeepSpeed #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411) — removes an invalid ZeRO gradient-norm fallback and adds a CPU regression test.
- [FlashAttention #2858](https://github.com/Dao-AILab/flash-attention/pull/2858) — slices FA4 backward-preprocess predicates per row for padded head dimensions.
- [Triton #11580](https://github.com/triton-lang/triton/pull/11580) — makes autotuner arguments thread-local and adds a deterministic concurrency test.
- [ONNX Runtime #32435](https://github.com/microsoft/onnxruntime/pull/32435) — prevents identity transposes from being folded into invalid Gemm `transB` rewrites.

</details>

No merge is claimed until an upstream repository accepts the change; detailed reproductions, commands, limitations, and review notes live in the [contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md).

## How I work

- Keep public projects runnable, scoped, and honest about limitations.
- Separate measured results from hypotheses and unfinished work.
- Prefer narrow diffs that are easy for maintainers to review.
- Record exact commands, environments, and reviewer feedback.

## Now

- **Studying:** Stanford CS336 and the systems behind modern LLMs
- **Building:** reproducible AI tools and experiments with Python/PyTorch
- **Contributing:** language-model systems, documentation, and reproducible engineering

## Contribution log

Short notes on upstream issues, reproductions, review feedback, and merged changes:

- [Open-source contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md)

## Tools

`Python` · `PyTorch` · `NumPy` · `TypeScript` · `Git` · `uv` · `pytest`

<div align="center">

<sub>Build the smallest version. Test the assumptions. Explain what happened.</sub>

</div>
