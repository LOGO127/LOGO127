# Open-source contribution log

This log records work that an upstream maintainer can verify. Status labels are intentionally conservative.

## 2026-09-04 — Hugging Face TRL PR #7038

- **Status:** PR open; PR template check passed; DCO-signed; maintainer review pending; not merged
- **Why this matters:** the Liger fused GRPO path cannot materialize router logits, so a Mixture-of-Experts auxiliary loss would otherwise be silently omitted from the training objective.
- **Scope:** add a GRPO fail-fast guard, document the `router_aux_loss_coef=0.0` requirement, and add an initialization regression test; DPO/KTO behavior is unchanged.
- **Validation:** Ruff check, Ruff format, and `git diff --check` passed; the focused test is skipped locally because `liger-kernel` is unavailable; manual tiny-MoE construction confirmed the expected `ValueError`.
- **Upstream link:** [Hugging Face TRL PR #7038](https://github.com/huggingface/trl/pull/7038)
- **Related issue:** [TRL issue #7009](https://github.com/huggingface/trl/issues/7009)
- **Review follow-up:** [design and validation note](https://github.com/huggingface/trl/pull/7038#issuecomment-5531032674)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — OpenAI Python PR #3790

- **Status:** PR open; DCO passed; maintainer review pending; not merged
- **Why this matters:** the streaming implementation contained an `sse.event == "error"` check inside a branch that only accepts events beginning with `thread.`, making that check unreachable.
- **Scope:** remove the duplicated unreachable branch from both `Stream` and `AsyncStream`; the existing reachable SSE error handling is unchanged.
- **Validation:** `tests/test_streaming.py` passed (20 tests); Ruff check, Ruff format, and `git diff --check` passed.
- **Upstream link:** [OpenAI Python PR #3790](https://github.com/openai/openai-python/pull/3790)
- **Related issue:** [openai-python issue #2796](https://github.com/openai/openai-python/issues/2796)
- **Review follow-up:** [SDK code-owner review request](https://github.com/openai/openai-python/pull/3790#issuecomment-5531142320)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — vLLM PR #55210

- **Status:** PR open; DCO passed; maintainer approval received; upstream CI/merge pending; not merged
- **Why this matters:** grouped streaming deltas could leak a reasoning start marker or drop content emitted after `</think>`.
- **Scope:** two reasoning-parser paths and focused regression coverage; no model-serving or GPU behavior changed.
- **Validation:** 41 reasoning tests passed; Ruff check, Ruff format, typos, and `git diff --check` passed.
- **Upstream link:** [vLLM PR #55210](https://github.com/vllm-project/vllm/pull/55210)
- **Review:** [maintainer approval and regression verification](https://github.com/vllm-project/vllm/pull/55210#pullrequestreview-5106377985)
- **CI follow-up:** [request for a write-access reviewer to trigger upstream CI](https://github.com/vllm-project/vllm/pull/55210#issuecomment-5535327497)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — Stanford CS336 lectures PR #47

- **Status:** PR open; review requested; not merged
- **Why this matters:** Lecture 11's caption described the optimal-batch curve in a way that reversed the fixed target-loss and minimized-token interpretation.
- **What I verified:** the replacement wording follows the construction described in MiniCPM Section 3.2 and Appendix A.2.
- **Scope:** one PDF slide; no source files or unrelated course assets changed.
- **Validation:** rendered all 58 pages before and after → only slide 11 changed; PDF page count, page size, and searchable replacement text verified.
- **Environment:** local PDF rendering and text-layer inspection; no CI checks are configured for this branch.
- **Upstream link:** [Stanford CS336 lectures PR #47](https://github.com/stanford-cs336/lectures/pull/47)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — SGLang PR #37935

- **Status:** PR open; mergeable; CI label/review pending; not merged
- **Why this matters:** DeepSeek-V4 KV-cache pools can allocate tensor-backed storage while the exported `kv_cache_memory_usage_gb` metric remains zero.
- **Scope:** account for single, indexer, unified, SWA/C4/C128, and compression-state buffers, including backend-specific indexer layouts.
- **Validation:** targeted tensor-accounting checks loaded the production source with CPU Torch; Ruff hook checks, formatting, compileall, and `git diff --check` passed. Full repository pytest collection is Linux/Triton-dependent and was not run on Windows.
- **Upstream link:** [SGLang PR #37935](https://github.com/sgl-project/sglang/pull/37935)
- **Related issue:** [SGLang issue #37852](https://github.com/sgl-project/sglang/issues/37852)
- **CI follow-up:** [request for a maintainer to enable the gated CI](https://github.com/sgl-project/sglang/pull/37935#issuecomment-5536229076)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — DeepSpeed PR #8411

- **Status:** PR open; DCO passed; CI/review pending; not merged
- **Why this matters:** the ZeRO CPU-offload gradient-norm path fell back to `param.grad` even when the configured gradient attribute was absent, although that source had already been moved and cleared.
- **Scope:** use the selected gradient attribute directly, fail clearly when it is missing, and cover both configured attribute paths plus the stale fallback regression.
- **Validation:** focused pytest passed (3 tests); DeepSpeed pre-commit hooks, compileall, and `git diff --check` passed.
- **Upstream link:** [DeepSpeed PR #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411)
- **Related issue:** [DeepSpeed issue #8371](https://github.com/deepspeedai/DeepSpeed/issues/8371)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — FlashAttention PR #2858

- **Status:** PR open; mergeable; upstream GPU CI/review pending; not merged
- **Why this matters:** FA4 backward preprocessing can fail to compile on padded head dimensions because each row copy receives a predicate with the unsliced tile shape.
- **Scope:** slice the head-dimension predicate with the same row index used for `O` and `dO` before both copies; the unpredicated path is unchanged.
- **Validation:** Ruff check, Ruff format, Python compilation, and `git diff --check` passed. CUDA/CuTe execution was not available locally on Windows and is explicitly left to upstream CI.
- **Upstream link:** [FlashAttention PR #2858](https://github.com/Dao-AILab/flash-attention/pull/2858)
- **Related issue:** [FlashAttention issue #2852](https://github.com/Dao-AILab/flash-attention/issues/2852)
- **Next action:** respond to maintainer feedback and revise only if requested

## Rules

- Never write “merged” until the upstream PR is actually merged.
- Paste exact commands and results; do not convert “not run” into “passed”.
- Link reviewer feedback and record how each requested change was addressed.
