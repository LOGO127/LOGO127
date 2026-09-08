# When a four-second queue budget expires after one second

Status as of September 8, 2026: [PR #248](https://github.com/vllm-project/router/pull/248)
submitted for [issue #247](https://github.com/vllm-project/router/issues/247)
after personal review and sign-off of commit `520303f`. DCO passed; Buildkite
build 791 is pending. The PR is open, not merged; no accepted fix or module
ownership is claimed. Investigation and implementation used Codex.

## Failure and bounded fix

On [baseline 0519da5](https://github.com/vllm-project/router/commit/0519da5397f429af9e54d1e9601b0a135e338b65),
the queue processor passes its remaining deadline to `acquire_timeout`.
That method wraps `acquire`, which has its own shorter deadline based on the
estimated next token refill. Under contention, the inner deadline can win
even when the configured queue budget would allow another refill.

The local candidate shares the existing polling loop, but applies only the
caller's deadline in the explicit-timeout API. Legacy `acquire` behavior,
public signatures and refill policy remain unchanged. This does not promise
FIFO scheduling or redesign admission and graceful shutdown.

## Evidence across three layers

| Check | Baseline | Local candidate |
| --- | --- | --- |
| Two token waiters, four-second budget | Both time out at about one second | Both obtain tokens at about two seconds |
| Production queue processor | Two 408 permit results | Both admitted |
| Loopback HTTP through production queue/middleware | Two HTTP 408 responses | Two HTTP 200 responses with complete bodies |

Controls retain short-deadline rejection, already-expired entry rejection,
accounting for time already spent queued, returned-token wakeup and dropped
waiter behavior. The HTTP terminal handler is synthetic, not a model backend.

## Validate the environment too

A shared Cargo target directory initially reused a candidate artifact during
the baseline HTTP check. That result was discarded. Visible baseline and
candidate recompilation established the HTTP comparison; final acceptance
used an independent target directory.

Rust 1.98.0 also reported lint warnings on unchanged baseline code. Checking
the pipeline identified its pinned version: **Rust 1.95.0**. On that version,
the independent candidate passed full formatting and strict all-target,
all-feature Clippy with warnings denied, without suppressions.

The same candidate then passed **540 tests**: 488 library cases, 47 existing
API endpoint cases, one added HTTP regression and four added token cases.
These counts are non-overlapping targets; the queue unit tests are already
included in the library count.

This is local Linux/WSL validation, not upstream CI, the full repository test
matrix, real GPU inference, performance measurement or evidence of adoption.
The next milestone is independent upstream CI and maintainer review.
