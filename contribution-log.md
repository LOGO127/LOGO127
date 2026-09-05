# Open-source contribution log

This log records work that an upstream maintainer can verify. Status labels are intentionally conservative.

## 2026-09-05 — OLMo-core PR #860

- **Status:** PR open; GitGuardian security check passed; maintainer review and any further upstream CI pending; not merged
- **Why this matters:** PyTorch's async checkpoint executor retains the submitted call arguments while future callbacks run, so a staged model and optimizer state can occupy substantial host memory even though later callbacks cannot use it.
- **Scope:** keep the staged top-level mapping available throughout the write, then clear it in the checkpointer's first completion callback before metadata writing and callbacks registered later by the trainer or caller; add a deterministic CPU regression and changelog entry.
- **Validation:** the regression failed against `origin/main` and passed with the patch (`1 passed`); an actual PyTorch async-checkpoint harness completed the save and observed the staged tensor collected before a later callback; isort, Black (`398 files would be left unchanged`), Ruff, full `mypy src/` (`397 source files`), and `git diff --check` passed.
- **Environment limitation:** the focused pytest invocation used a local Windows-only import shim for `bettermap`'s POSIX `fork` assumption; the shim is not part of the submitted change, and upstream Linux CI remains authoritative.
- **Upstream link:** [OLMo-core PR #860](https://github.com/allenai/OLMo-core/pull/860)
- **Related issue:** [OLMo-core issue #856](https://github.com/allenai/OLMo-core/issues/856)
- **Next action:** monitor maintainer review and upstream CI; revise only in response to concrete failures or feedback.

## 2026-09-05 — PrimeRL PR #3489

- **Status:** PR open, mergeable, and ready for review; Style, CPU, and GPU workflows await the repository's external-contributor approval; maintainer review pending; not merged
- **Why this matters:** W&B 0.27.1 removed the `wandb_gql` package, so importing PrimeRL's W&B overview monitor could fail before training started on current W&B releases.
- **Scope:** replace the direct `wandb_gql` dependency with the GraphQL compatibility transport already provided by the project's required `wandb-workspaces>=0.4.3`, keep the query as a raw string, and add a focused unit regression test.
- **Validation:** the focused regression test passed under both W&B 0.27.0 and 0.28.2 (`1 passed` per environment); separate no-network smokes exercised the real legacy-client and service-API transports; Ruff lint, Ruff formatting, and `git diff --check` passed.
- **Environment limitation:** the repository's full dependency resolution targets Linux/macOS and did not complete on Windows, so the full project suite was not run locally; the focused pytest runs isolated unrelated package initializers, and upstream Ubuntu CI remains pending.
- **Upstream link:** [PrimeRL PR #3489](https://github.com/PrimeIntellect-ai/prime-rl/pull/3489)
- **Related issue:** [PrimeRL issue #3087](https://github.com/PrimeIntellect-ai/prime-rl/issues/3087)
- **Next action:** wait for a maintainer to approve the external-contributor workflows, then fix any concrete CI failures and respond to review.

## 2026-09-05 — OLMo-core PR #858

- **Status:** PR open; maintainer review and upstream CI pending; not merged
- **Why this matters:** `BeakerCallback.post_attach()` only needs to inspect the runtime environment, but it imported the full Beaker launch module and therefore failed when the optional `beaker` or `gantry` packages were absent.
- **Scope:** move Beaker environment detection to the dependency-light launch utilities, re-export the helpers from their existing module for compatibility, and keep explicit Beaker operations unchanged.
- **Validation:** environment-matrix and callback regression tests passed (7 tests total); the callback test explicitly blocked the optional launch module; Ruff, Black, isort, focused mypy, compileall, and `git diff --check` passed.
- **Environment limitation:** the callback test used a local-only spawn compatibility shim because the repository's `bettermap` dependency requires a Unix `fork` context; no shim is part of the submitted change.
- **Upstream link:** [OLMo-core PR #858](https://github.com/allenai/OLMo-core/pull/858)
- **Related issue:** [OLMo-core issue #850](https://github.com/allenai/OLMo-core/issues/850)
- **Next action:** monitor upstream CI and maintainer review; revise only in response to concrete failures or feedback.

## 2026-09-05 — bitsandbytes PR #2073

- **Status:** PR open and mergeable; upstream CI and maintainer review pending; not merged
- **Why this matters:** production ROCm containers may intentionally omit `rocminfo`, leaving bitsandbytes unable to identify the active `gfx` architecture even when the operator already knows it.
- **Scope:** accept a validated runtime `BNB_ROCM_ARCH` override before the existing tool probe, normalize feature-qualified architecture strings, preserve the current fallback when the variable is absent, and document the distinction from the build-time CMake option.
- **Validation:** `tests/test_cuda_setup_evaluator.py` passed (13 tests); focused Ruff lint and formatting checks passed; all repository pre-commit hooks passed for the three changed files; `git diff --check` passed.
- **Upstream link:** [bitsandbytes PR #2073](https://github.com/bitsandbytes-foundation/bitsandbytes/pull/2073)
- **Related issue:** [bitsandbytes issue #1444](https://github.com/bitsandbytes-foundation/bitsandbytes/issues/1444)
- **Next action:** monitor upstream CI and maintainer review; revise only in response to concrete failures or feedback.

## 2026-09-05 — GPT-NeoX PR #1419

- **Status:** PR open; CLA signed by the contributor and `license/cla` check passed; maintainer review pending; not merged
- **Why this matters:** the pull-request test job ran `prepare_data.py` before installing PyTorch and the project's dataset dependencies, so a clean runner could fail before dependency installation.
- **Scope:** move only the existing data-preparation step after the pytest, PyTorch, and project-requirements installation steps, matching the order already used by the CPU CI workflow.
- **Validation:** all repository pre-commit hooks passed for the workflow file; the YAML parsed successfully; an explicit ordering assertion confirmed dependency installation precedes data preparation, which precedes the test invocation.
- **Upstream link:** [GPT-NeoX PR #1419](https://github.com/EleutherAI/gpt-neox/pull/1419)
- **Related issue:** [GPT-NeoX issue #1413](https://github.com/EleutherAI/gpt-neox/issues/1413)
- **CLA follow-up:** the contributor completed the agreement personally; the bot-provided recheck refreshed `license/cla` to SUCCESS on 2026-09-05 at 03:06 UTC. No further signature is currently requested.
- **Next action:** monitor maintainer review and any upstream CI; revise only in response to concrete feedback.

## 2026-09-05 — LLaMA-Factory PR #10813

- **Status:** PR open and mergeable; maintainer review and upstream CI pending; not merged
- **Why this matters:** DeepSpeed ZeRO-1/2 clones tensors while gathering a full state dict, breaking the storage alias between tied input embeddings and `lm_head`; Transformers then serializes both copies even when `tie_word_embeddings=true`.
- **Scope:** recover aliases from the model's actual shared `Parameter` objects before the SFT trainer saves an externally supplied state dict; leave genuinely untied parameters unchanged.
- **Validation:** download-free tiny-Qwen3 regression and negative tests passed on both Transformers 4.55.0 and 5.8.0 (2 tests per version); all applicable pre-commit hooks and the repository license check passed.
- **Upstream link:** [LLaMA-Factory PR #10813](https://github.com/hiyouga/LlamaFactory/pull/10813)
- **Related issue:** [LLaMA-Factory issue #10560](https://github.com/hiyouga/LlamaFactory/issues/10560)
- **Next action:** monitor upstream CI and maintainer review; revise only in response to concrete failures or feedback.

## 2026-09-05 — vLLM-Omni PR #7065

- **Status:** **Merged upstream** on 2026-09-04 at 23:06 UTC after collaborator approval; DCO, Python 3.11/3.12 wheel builds, pre-commit, generic Buildkite, Intel CI, NPU CI, and Read the Docs passed; AMD CI's unrelated SenseNova failure was reviewed and treated as non-blocking
- **Why this matters:** vLLM 0.28.0 correctly rejects negative prompt token IDs, but Higgs Audio v3 voice cloning used `-100` sentinels for reference-audio embeddings, so requests failed before reaching the model.
- **Scope:** replace model-local sentinels with a vocabulary-valid non-audio filler before engine submission, carry their absolute prompt positions as tensor metadata, map reference-code rows correctly during full or chunked prefill, retain the legacy internal fallback, and keep the existing offline example consistent.
- **Validation:** four targeted CPU regression tests passed for vocabulary-valid prompt preparation, full prefill, chunked prefill, and the legacy fallback; an isolated online-serving prompt smoke check passed; all applicable non-mypy pre-commit hooks passed. The current upstream files contain pre-existing mypy failures, and the H100/model E2E test was not runnable locally on Windows.
- **Upstream link:** [vLLM-Omni PR #7065](https://github.com/vllm-project/vllm-omni/pull/7065)
- **Related issue:** [vLLM-Omni issue #6837](https://github.com/vllm-project/vllm-omni/issues/6837)
- **Review follow-up:** [author self-review and automated-review request](https://github.com/vllm-project/vllm-omni/pull/7065#issuecomment-5545802035); ReviewBot classified the fix as high priority; the collaborator's filler-token concern was [addressed with a non-audio token and stronger assertions](https://github.com/vllm-project/vllm-omni/pull/7065#issuecomment-5547166094); collaborator `linyueqian` then [approved the final commit](https://github.com/vllm-project/vllm-omni/pull/7065#pullrequestreview-5118503539) and merged it
- **Outcome:** 190 additions and 37 deletions across six files; regression coverage ran in upstream CI (`1787 passed, 1 skipped, 610 deselected` in the relevant lane).

## 2026-09-05 — Ollama PR #18234

- **Status:** PR open; mergeable; maintainer review pending; not merged
- **Why this matters:** the Linux manual-install instructions required `sudo`, even though the release archive can run correctly from a user-writable prefix for developers without root access.
- **Scope:** document extraction under `$HOME/.local`, adding the user-local binary directory to `PATH`, and the matching ARM64 and AMD ROCm archive variants.
- **Validation:** `git diff --check` passed; the branch was synchronized with current upstream `main` before submission. This is a documentation-only change, and the repository test workflow excludes `docs/**`.
- **Upstream link:** [Ollama PR #18234](https://github.com/ollama/ollama/pull/18234)
- **Related issue:** [Ollama issue #18215](https://github.com/ollama/ollama/issues/18215)
- **Next action:** wait for maintainer review and revise only if requested.

## 2026-09-05 — Faiss PR #5574

- **Status:** PR open; Meta CLA passed; local native generic/AVX2 tests passed; Meta import queued; review pending; not merged
- **Why this matters:** `METRIC_Canberra` evaluated `0 / 0` when both vectors were zero in a dimension, producing `NaN` and causing `IndexFlat.search` to retain `FLT_MAX/-1` empty-result sentinels.
- **Scope:** skip the zero-denominator term so it contributes `0`, and add regression coverage for both `pairwise_distances` and `IndexFlat.search`.
- **Validation:** built the actual C++ library and SWIG bindings in WSL Ubuntu (GCC 13.3, C++20, Release, CPU-only), verifying the loaded generic and AVX2 modules separately. Before the fix, the new regression failed in both native builds with `NaN` distances and `FLT_MAX/-1` search results. After the fix, `tests/test_extra_distances.py` passed all **14 tests in each build**. Additional SciPy comparisons passed for eight dimensions with sparse signed vectors, shared zeros, and partial/full-k searches. Both changed source files matched the submitted files by SHA-256; Python syntax, focused Ruff lint, and `git diff --check` passed. GPU and Meta's internal suite were not run locally.
- **Upstream link:** [Faiss PR #5574](https://github.com/facebookresearch/faiss/pull/5574)
- **Related issue:** [Faiss issue #5557](https://github.com/facebookresearch/faiss/issues/5557)
- **CI:** [Meta CLA check](https://github.com/facebookresearch/faiss/pull/5574) and Meta import checks passed; Import Status remains queued. Local native test results are not a claim of internal CI success.
- **Next action:** native validation evidence added to the PR description; wait for Meta's import/internal validation and maintainer review.

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

- **Status:** PR open; follow-up fix and stronger regression coverage pushed in `ffa9975` on 2026-09-05; prior maintainer approval was dismissed after earlier updates, so upstream CI and re-review remain pending; Microsoft CLA passed after the author signed personally; not merged
- **Why this matters:** `GemmTransposeFusion` treated an identity `Transpose(perm=[0, 1])` as a matrix transpose, removed it, and changed `transB`, which can produce an invalid Gemm bias shape.
- **Scope:** require a real two-dimensional matrix transpose before folding input or output Transpose nodes into Gemm, including the `Apply` output branch when a real input transpose triggers fusion; preserve default reverse-permutation behavior for known 2D inputs; check non-default Gemm attributes and A/B input order, and cover mixed real-input/identity-output transposes on either input.
- **Validation:** built the real `onnxruntime_test_all` executable in WSL Ubuntu with GCC 13.3, CMake 3.28.3, CPU-only Release. The new mixed-path regression failed on the previous PR head `031cc67` (output shape changed from `3x5` to `5x3`, and the identity output node was lost), while the other 8 targeted tests passed. After `ffa9975`, `GraphTransformationTests.GemmTransposeFusion*` passed all 9 tests; `GraphTransformationTests.*` reported 402 passed and 15 skipped, with no failures. The skipped cases require unavailable CUDA (2) or WebGPU (13) execution providers. Repository CLANGFORMAT lint and `git diff --check` passed. The native build's two changed source files matched the submitted files by SHA-256; upstream platform CI is still pending.
- **CI failure and fix:** [first full-build failure](https://github.com/microsoft/onnxruntime/actions/runs/33845286359/job/101002854462) · [fix commit](https://github.com/LOGO127/onnxruntime/commit/031cc67)
- **Upstream link:** [ONNX Runtime PR #32435](https://github.com/microsoft/onnxruntime/pull/32435)
- **Related issue:** [ONNX Runtime issue #32418](https://github.com/microsoft/onnxruntime/issues/32418)
- **Maintainer review:** [approval and follow-up question](https://github.com/microsoft/onnxruntime/pull/32435#pullrequestreview-5112190255)
- **Audit follow-up:** [review response on related transpose paths](https://github.com/microsoft/onnxruntime/pull/32435#issuecomment-5539750598)
- **Follow-up correction and native test evidence:** [review-thread reply](https://github.com/microsoft/onnxruntime/pull/32435#discussion_r3939156337). The earlier output guard did not cover `Apply` when an input transpose triggered the rule; `ffa9975` closes that gap.
- **CLA confirmation:** [author's agreement comment](https://github.com/microsoft/onnxruntime/pull/32435#issuecomment-5548587260); `license/cla` completed successfully at 2026-09-05 01:59:27 UTC.
- **Next action:** monitor upstream CI and maintainer re-review; repair concrete failures or address new feedback without duplicate review requests

## Rules

- Never write “merged” until the upstream PR is actually merged.
- Paste exact commands and results; do not convert “not run” into “passed”.
- Link reviewer feedback and record how each requested change was addressed.
