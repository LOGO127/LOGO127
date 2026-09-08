# Validating a parser-history performance regression

Status on September 8, 2026: **tested fork candidate; upstream coordination pending**.
This is not a merged contribution or an assigned maintainer role.

## Attribution and contribution

[thincal's XGrammar issue #873](https://github.com/mlc-ai/xgrammar/issues/873)
identified the exact-reserve regression and proposed geometric growth. My contribution,
prepared with Codex assistance, is independent validation, deterministic regression
tests, and an attributed [two-file candidate](https://github.com/LOGO127/xgrammar/commit/4e4d55b17c22fe8101b000d80d8de71c9c3aa96b).
The [coordination comment](https://github.com/mlc-ai/xgrammar/issues/873#issuecomment-5579765226)
offers the tests or candidate for the reporter's preferred contribution route.

## Failure mechanism

The affected preview parser appends rows of state through `PushBackIndirect`.
Reserving exactly the next required size repeatedly relocates the retained history.
Speculative token checks can cross the previous capacity even when their rows are
subsequently rolled back. Geometric growth avoids repeated full-history moves.
The candidate is based on rc3 `07cb2df`, not main, which lacks this path.

## Evidence

- A copy/move-count regression fails on unchanged rc3, with 524,800 and 528,897
  operations for 1,024 retained rows, against a generous 10,240 linear bound.
- Both new cases pass with ASan/UBSan; they cover append/pop, empty rows,
  multiple elements, and regrowth. The complete configured C++ target passes 108 tests.
- Related Python tests pass on both versions: 20 passed, 12 HF_TOKEN-gated skips.
- The reporter's exact grammar and seeded 32,000-entry vocabulary were run for
  20,000 tokens. Every mask was compared bytewise between independent original and
  patched native processes, including ten-token replay and a branching draft tree:
  **80,052,000 bytes matched**. The clean-vocabulary control matched separately.

| Final 2,000-token window | Original mean fill | Patched mean fill |
| --- | --- | --- |
| Trigger vocabulary | 13,711.9 µs | 3.22 µs |
| Clean control | 0.962 µs | 1.069 µs |

These are single local Linux CPU synthetic runs, not a universal speedup or a
production serving benchmark. Mask copying was outside the timed call. Native
libraries were compiled with GNU 13.3 / Release; source-library discovery was
explicitly selected to avoid an unrelated installed wheel. Manual FFI stub generation
was a local build workaround, not part of this patch. No GPU, real model, production
150k-token vocabulary, or general destination-alias safety result is claimed.

## Engineering lesson

Use deterministic operation counts to catch complexity regressions, then separately
verify full output equivalence and measure the affected workload. Keep original
discovery, independent verification, public code delivery, and upstream acceptance
distinct when reporting the contribution.
