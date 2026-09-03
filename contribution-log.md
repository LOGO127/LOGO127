# Open-source contribution log

This log records work that an upstream maintainer can verify. Status labels are intentionally conservative.

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
