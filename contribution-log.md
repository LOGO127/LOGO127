# Open-source contribution log

This log records work that an upstream maintainer can verify. Status labels are intentionally conservative.

## 2026-09-05 — Faiss PR #5574

- **Status:** PR open; mergeable; Meta CLA signed and check passed; compiled CI queued; review pending; not merged
- **Why this matters:** `METRIC_Canberra` evaluated `0 / 0` when both vectors were zero in a dimension, producing `NaN` and causing `IndexFlat.search` to retain `FLT_MAX/-1` empty-result sentinels.
- **Scope:** skip the zero-denominator term so it contributes `0`, and add regression coverage for both `pairwise_distances` and `IndexFlat.search`.
- **Validation:** reproduced the reported behavior with the installed Faiss 1.12.0 package; Python test syntax, targeted Ruff checks, and `git diff --check` passed. A full C++ build was not available locally because the Windows environment lacks CMake/C++ tooling.
- **Upstream link:** [Faiss PR #5574](https://github.com/facebookresearch/faiss/pull/5574)
- **Related issue:** [Faiss issue #5557](https://github.com/facebookresearch/faiss/issues/5557)
- **CI:** [Meta CLA check](https://github.com/facebookresearch/faiss/pull/5574) passed; Meta import checks passed and the compiled import status remains queued.
- **Next action:** wait for compiled CI and maintainer review.

## 2026-09-05 — PyTorch Geometric PR #10797

- **Status:** PR open; mergeable; changelog follow-up pushed; Read the Docs and pre-commit CI passed; review pending; not merged
- **Why this matters:** the ONNX-specific `min`/`max` scatter path passed a zero-dimensional Tensor as `torch.full`'s `fill_value`, which older PyTorch versions reject during ONNX export.
- **Scope:** preserve the dynamically computed fill value with `reshape(1).expand_as(src)`, keeping dtype/device behavior and avoiding conversion to a Python scalar; add regression coverage for both reductions.
- **Validation:** targeted ONNX-path tests passed (2); the complete scatter test file passed (23 passed, 10 skipped because optional `torch-scatter` is unavailable); link-pred metric tests passed (16); Ruff lint and `git diff --check` passed. The current Ruff formatter reports pre-existing formatting differences in these files, so no broad reformat was applied.
- **Upstream link:** [PyTorch Geometric PR #10797](https://github.com/pyg-team/pytorch_geometric/pull/10797)
- **Related issue:** [PyTorch Geometric issue #10327](https://github.com/pyg-team/pytorch_geometric/issues/10327)
- **CI:** [Read the Docs preview](https://pytorch-geometric--10797.org.readthedocs.build/en/10797/) and [pre-commit.ci run](https://results.pre-commit.ci/run/github/106024057/1788543530.WREly60SS52MXPwhX-0wqA) passed after commit `892a11a`
- **Review follow-up:** [CI-green readiness note](https://github.com/pyg-team/pytorch_geometric/pull/10797#issuecomment-5544406639) plus a single [code-owner mention](https://github.com/pyg-team/pytorch_geometric/pull/10797#issuecomment-5544449552); formal reviewer requests are unavailable to this account
- **Next action:** wait for CI and maintainer review; revise only if requested

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

- **Status:** PR open; mergeable; DCO passed; maintainer confirmed the cleanup is correct; formal code-owner approval pending; not merged
- **Why this matters:** the streaming implementation contained an `sse.event == "error"` check inside a branch that only accepts events beginning with `thread.`, making that check unreachable.
- **Scope:** remove the duplicated unreachable branch from both `Stream` and `AsyncStream`; the existing reachable SSE error handling is unchanged.
- **Validation:** `tests/test_streaming.py` passed (20 tests); Ruff check, Ruff format, and `git diff --check` passed.
- **Upstream link:** [OpenAI Python PR #3790](https://github.com/openai/openai-python/pull/3790)
- **Related issue:** [openai-python issue #2796](https://github.com/openai/openai-python/issues/2796)
- **Maintainer review:** [positive semantic review](https://github.com/openai/openai-python/pull/3790#pullrequestreview-5115251602)
- **Review follow-up:** [formal code-owner approval request](https://github.com/openai/openai-python/pull/3790#issuecomment-5543582043)
- **Next action:** wait for `openai/sdks-team` approval; revise only if maintainers request changes

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

- **Status:** PR open; DCO and remote CI passed; maintainer review pending; not merged
- **Why this matters:** the ZeRO CPU-offload gradient-norm path fell back to `param.grad` even when the configured gradient attribute was absent, although that source had already been moved and cleared.
- **Scope:** use the selected gradient attribute directly, fail clearly when it is missing, and cover both configured attribute paths plus the stale fallback regression.
- **Validation:** focused pytest passed (3 tests); DeepSpeed pre-commit hooks, compileall, and `git diff --check` passed.
- **Upstream link:** [DeepSpeed PR #8411](https://github.com/deepspeedai/DeepSpeed/pull/8411)
- **Related issue:** [DeepSpeed issue #8371](https://github.com/deepspeedai/DeepSpeed/issues/8371)
- **CI follow-up:** [remote DeepSpeedAI CI run](https://github.com/deepspeedai/DeepSpeed/actions/runs/33842132211) completed successfully; [ready-for-review note](https://github.com/deepspeedai/DeepSpeed/pull/8411#issuecomment-5537765677); [single gentle follow-up](https://github.com/deepspeedai/DeepSpeed/pull/8411#issuecomment-5543535064)
- **Next action:** wait for maintainer feedback and revise only if requested

## 2026-09-04 — FlashAttention PR #2858

- **Status:** PR open; mergeable; upstream GPU CI/review pending; not merged
- **Why this matters:** FA4 backward preprocessing can fail to compile on padded head dimensions because each row copy receives a predicate with the unsliced tile shape.
- **Scope:** slice the head-dimension predicate with the same row index used for `O` and `dO` before both copies; the unpredicated path is unchanged.
- **Validation:** Ruff check, Ruff format, Python compilation, and `git diff --check` passed. CUDA/CuTe execution was not available locally on Windows and is explicitly left to upstream CI.
- **Upstream link:** [FlashAttention PR #2858](https://github.com/Dao-AILab/flash-attention/pull/2858)
- **Related issue:** [FlashAttention issue #2852](https://github.com/Dao-AILab/flash-attention/issues/2852)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — Triton PR #11580

- **Status:** PR open; mergeable; upstream CI/review pending; not merged
- **Why this matters:** concurrent autotuning of the same kernel could let one thread clear the shared `nargs` field while another thread was still benchmarking, causing a `NoneType` mapping error.
- **Scope:** store `Autotuner.nargs` in thread-local storage and add a deterministic regression test that overlaps two cold-cache autotuning runs for the same key.
- **Validation:** Triton’s fixed pre-commit hooks passed, including Ruff, YAPF, mypy, AST, and secret checks; Python compilation and `git diff --check` passed. Targeted pytest was not run locally because this Windows checkout lacks Triton’s compiled `_C` extension and CUDA runtime.
- **Upstream link:** [Triton PR #11580](https://github.com/triton-lang/triton/pull/11580)
- **Related issue:** [Triton issue #11494](https://github.com/triton-lang/triton/issues/11494)
- **Next action:** respond to maintainer feedback and revise only if requested

## 2026-09-04 — ONNX Runtime PR #32435

- **Status:** PR open; mergeable; maintainer approval was received on the prior commit; CI exposed a test compile error and the fix was pushed in `031cc67`; GitHub dismissed the prior approval after the new commit, so re-review is pending; CLA pending; not merged
- **Why this matters:** `GemmTransposeFusion` treated an identity `Transpose(perm=[0, 1])` as a matrix transpose, removed it, and changed `transB`, which can produce an invalid Gemm bias shape.
- **Scope:** require a real two-dimensional matrix transpose before folding input or output Transpose nodes into Gemm, while preserving default reverse-permutation behavior for known 2D inputs; add graph-transform regression tests for both input- and output-side identity transposes.
- **Validation:** ORT lintrunner passed for both changed C++ files, including clang-format; `git diff --check` passed; the first upstream full-build batch consistently failed at the four new shape-only `MakeInput` calls, which was corrected in `031cc67` using the repository's existing double-brace shape form; a CPU-wheel baseline reproducer on ORT 1.22.1 showed optimization-disabled success and basic-optimization failure with `transB=1`. The source C++ gtest was not run locally because no ORT C++ build exists in this Windows checkout.
- **CI failure and fix:** [first full-build failure](https://github.com/microsoft/onnxruntime/actions/runs/33845286359/job/101002854462) · [fix commit](https://github.com/LOGO127/onnxruntime/commit/031cc67)
- **Upstream link:** [ONNX Runtime PR #32435](https://github.com/microsoft/onnxruntime/pull/32435)
- **Related issue:** [ONNX Runtime issue #32418](https://github.com/microsoft/onnxruntime/issues/32418)
- **Maintainer review:** [approval and follow-up question](https://github.com/microsoft/onnxruntime/pull/32435#pullrequestreview-5112190255)
- **Audit follow-up:** [review response on related transpose paths](https://github.com/microsoft/onnxruntime/pull/32435#issuecomment-5539750598)
- **Next action:** complete the repository CLA personally if its terms accurately apply, then monitor the rerun CI and request/await fresh maintainer review

## Rules

- Never write “merged” until the upstream PR is actually merged.
- Paste exact commands and results; do not convert “not run” into “passed”.
- Link reviewer feedback and record how each requested change was addressed.
