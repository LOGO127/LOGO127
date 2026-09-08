# A conversation dataset should survive file replay

September 8, 2026 · AI benchmarking reliability · **local patch, not an accepted fix**

Public evidence and proposed scope: [GuideLLM #1024 discussion](https://github.com/vllm-project/guidellm/issues/1024#issuecomment-5586175103).
The issue was opened by another contributor; the reproduction and local candidate
described here were developed with Codex assistance. Personal review and DCO are
pending. No assignment or module ownership is claimed.

## Failure at the data boundary

Current GuideLLM already maps `conversation_turns` and accepts nested graph data.
The problem is more specific than “JSONL replay is unsupported”: its Arrow-backed
loader aligns fields across rows and turns, inserting nulls for missing columns.

A root with only `text_column` and a child with an additional
`output_tokens_count_column: [7]` becomes a root with
`output_tokens_count_column: null`. The request finalizer expects to iterate a
sequence and raises `TypeError`. Prefix and media column containers fail similarly.

The candidate normalizes only null containers directly inside
`ConversationTurnData.columns`. Nested tool nulls, list entries, empty lists and
zero counts remain unchanged. This is an explicit normalization policy, not a
claim of lossless conversion for every possible nested Arrow schema.

## Matched evidence, not just a passing test

Baseline: [`fc2dbe9`](https://github.com/vllm-project/guidellm/commit/fc2dbe9edd4f7f1a4e9ccd752f6f43591adbcb73).

| Check | Unmodified baseline | Local candidate |
| --- | --- | --- |
| Same 16 JSONL cases | 8 fail in the real finalizer; 8 pass | 16 pass |
| Exact two-turn JSONL through CLI | Row skipped; process exits with no available samples | Two successful HTTP requests |
| Child history and output limit | No request can be constructed | Parent history retained; child limit remains 7 tokens |

The integration cases cover two graph rows, streaming/nonstreaming file loading,
nested/string payloads, request columns, agent IDs, scheduling settings and history
edges. The CLI probe uses the actual loader, scheduler and HTTP backend with a
localhost MockServer and offline vendored tokenizer—not a substitute data pipeline.

Broader candidate checks: **3,032 unit tests passed** (31 skipped, 163 expected
failures), **44 integration tests passed** (25 expected failures), and **14 default
E2E tests passed**. Configured lint, import contracts and source type checks pass.
Both baseline and candidate integration runs print a separate multiprocessing
`MagicMock` serialization diagnostic while exiting successfully; this patch does
not repair that diagnostic. These test counts overlap smaller checks and are not
additive evidence.

## Limits and next step

These are local CPU/software checks, not upstream CI, GPU inference, model-quality
or throughput results. The exact-fixture CLI probe is separate from the four-file
review candidate. The proposed policy was shared with maintainers; no PR or accepted
fix is claimed. Review the behavior and contributor attestation before publication.
