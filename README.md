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

## Open-source track

I prefer the engineering loop:

**reproduce → understand the contract → make the smallest useful change → test → explain the evidence**

Current upstream contributions:

- [Hugging Face TRL PR #7038](https://github.com/huggingface/trl/pull/7038) — adds a fail-fast guard for the incompatible MoE auxiliary-loss and Liger GRPO combination.
- [OpenAI Python PR #3790](https://github.com/openai/openai-python/pull/3790) — removes unreachable SSE error checks from the synchronous and asynchronous streaming paths.
- [vLLM PR #55210](https://github.com/vllm-project/vllm/pull/55210) — fixes reasoning-parser boundaries when streaming deltas cross `<think>` / `</think>` markers.
- [Stanford CS336 lectures PR #47](https://github.com/stanford-cs336/lectures/pull/47) — clarifies the optimal-batch caption in Lecture 11.
- [SGLang PR #37935](https://github.com/sgl-project/sglang/pull/37935) — reports tensor-backed DeepSeek-V4 KV-cache memory usage across pooled storage layouts.
- [DeepSpeed PR #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411) — removes an invalid ZeRO gradient-norm fallback and adds a CPU regression test.

Six PRs are open across model training, inference, serving, SDK, and course infrastructure. vLLM #55210 has maintainer approval and is waiting for upstream CI and merge; the other PRs are awaiting review or CI as applicable. No merge is claimed until the upstream repositories accept the changes.

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
