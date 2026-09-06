<div align="center">

<img src="https://raw.githubusercontent.com/LOGO127/LOGO127/main/assets/header.svg" alt="LOGO127 — AI Systems, LLM Training, and Inference" width="100%" />

# Zijian Luo · LOGO127

AI systems first · AI for Science alongside

Master's student at Zhejiang University. I build reproducible systems to understand
language models, with a growing focus on reliable inference and stateful agent APIs.

[Projects](https://github.com/LOGO127?tab=repositories) · [Contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md) · [Upstream PRs](https://github.com/pulls?q=is%3Apr+author%3ALOGO127+-user%3ALOGO127)

</div>

---

## ✅ Merged contribution

[vLLM-Omni #7065](https://github.com/vllm-project/vllm-omni/pull/7065) fixes Higgs Audio v3
voice-clone token validation while preserving reference-audio placement across chunked
prefill. Merged after collaborator review on September 4, 2026 (UTC).[^1]

## 🎯 Current contribution focus

| Track | Engineering focus | Public work |
| --- | --- | --- |
| **Agentic API** | Bounded response sessions; continuation and storage contracts | [#257](https://github.com/vllm-project/agentic-api/pull/257) — draft; [#258](https://github.com/vllm-project/agentic-api/pull/258) — open[^2] |
| **vLLM-Omni** | Audio diagnostics; request lifecycle and timeout correctness | [#7098](https://github.com/vllm-project/vllm-omni/pull/7098), [#7151](https://github.com/vllm-project/vllm-omni/pull/7151) — open[^3] |
| **DeePMD-kit** | Ragged graph batching; charge/spin and numerical correctness | [#6008](https://github.com/deepmodeling/deepmd-kit/pull/6008), [#6010](https://github.com/deepmodeling/deepmd-kit/pull/6010) — open[^4] |
| **MACE** | Scientific model-download reliability | [#1712](https://github.com/ACEsuit/mace/pull/1712) — open[^5] |

These are contribution areas I am working toward maintaining, not assigned module
ownership. Open and draft PRs are not accepted contributions. The
[dated contribution log](https://github.com/LOGO127/LOGO127/blob/main/contribution-log.md)
separates merged work, review candidates, and unpublished experiments.

## 📦 Personal projects

| Project | Focus |
| --- | --- |
| [wechat-ai-memory](https://github.com/LOGO127/wechat-ai-memory) | Local-first, traceable conversation context |
| [CS336 Assignment 1](https://github.com/LOGO127/cs336-2026-assignment1-llm-foundations) | From-scratch language-model foundations |
| [cs336.2026](https://github.com/LOGO127/cs336.2026) | Systems-first learning and implementation notes |

## 🔍 Engineering approach

- Reproduce failures and preserve passing controls before changing behavior.
- Keep patches scoped, with explicit dependency and compatibility boundaries.
- Separate local tests, upstream CI, and actual model/hardware validation.
- Disclose AI assistance and respond to review with reproducible evidence.

`Python` · `Rust` · `PyTorch` · `pytest` · `Git` · `uv`

---

<sub>Build the smallest useful version. Test the assumptions. Explain what happened.</sub>

[^1]: vLLM-Omni. [Merged PR #7065](https://github.com/vllm-project/vllm-omni/pull/7065).
[^2]: Agentic API. [Core session draft #257](https://github.com/vllm-project/agentic-api/pull/257), [typed Responses file validation #258](https://github.com/vllm-project/agentic-api/pull/258). Status checked September 6, 2026, 20:59 UTC; no WebSocket integration or module ownership is claimed.
[^3]: vLLM-Omni. [Reference-audio diagnostics #7098](https://github.com/vllm-project/vllm-omni/pull/7098), [shared RPC deadline #7151](https://github.com/vllm-project/vllm-omni/pull/7151). Open at the same check.
[^4]: DeePMD-kit. [Ragged charge/spin batching #6008](https://github.com/deepmodeling/deepmd-kit/pull/6008), [padding-safe force/Hessian metrics #6010](https://github.com/deepmodeling/deepmd-kit/pull/6010). Open at the same check.
[^5]: MACE. [Reject HTML before caching model downloads #1712](https://github.com/ACEsuit/mace/pull/1712). Open at the same check.
