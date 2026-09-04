<div align="center">

# LOGO127 👋

### AI Systems · LLM Foundations · AI for Science

Master's student at Zhejiang University. I build small, reproducible systems
to understand language models and turn real engineering problems into clear,
reviewable open-source work.

[Projects](https://github.com/LOGO127?tab=repositories) · [Contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md)

</div>

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
| **Training systems** | [DeepSpeed #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411) — ZeRO gradient-norm fallback | Open; remote test running |
| **Runtime correctness** | [ONNX Runtime #32435](https://github.com/microsoft/onnxruntime/pull/32435) — identity transpose/Gemm fusion | Open; regression coverage added; CLA pending |
| **Kernel/compiler work** | [FlashAttention #2858](https://github.com/Dao-AILab/flash-attention/pull/2858) · [Triton #11580](https://github.com/triton-lang/triton/pull/11580) | Open; hardware CI/review pending |

<details>
<summary>Full upstream contribution log · 9 open PRs</summary>

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
