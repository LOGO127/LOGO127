# Open-source contribution log

This log records work that an upstream maintainer can verify. Status labels are intentionally conservative.

## 2026-09-04 — OpenAI Python PR #3790

- **Status:** PR open; DCO passed; maintainer review pending; not merged
- **Why this matters:** the streaming implementation contained an `sse.event == "error"` check inside a branch that only accepts events beginning with `thread.`, making that check unreachable.
- **Scope:** remove the duplicated unreachable branch from both `Stream` and `AsyncStream`; the existing reachable SSE error handling is unchanged.
- **Validation:** `tests/test_streaming.py` passed (20 tests); Ruff check, Ruff format, and `git diff --check` passed.
- **Upstream link:** [OpenAI Python PR #3790](https://github.com/openai/openai-python/pull/3790)
- **Related issue:** [openai-python issue #2796](https://github.com/openai/openai-python/issues/2796)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — vLLM PR #55210

- **Status:** PR open; DCO passed; maintainer review pending; not merged
- **Why this matters:** grouped streaming deltas could leak a reasoning start marker or drop content emitted after `</think>`.
- **Scope:** two reasoning-parser paths and focused regression coverage; no model-serving or GPU behavior changed.
- **Validation:** 41 reasoning tests passed; Ruff check, Ruff format, typos, and `git diff --check` passed.
- **Upstream link:** [vLLM PR #55210](https://github.com/vllm-project/vllm/pull/55210)
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

## Rules

- Never write “merged” until the upstream PR is actually merged.
- Paste exact commands and results; do not convert “not run” into “passed”.
- Link reviewer feedback and record how each requested change was addressed.
