---
type: tool
topics:
  - ai-coding-tools
  - ai-agent-orchestration
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# OpenAI Codex (as the Main Agent Brain)

## Summary

In this setup, OpenAI Codex is used as the "main Hermes brain" — handling general back-and-forth
coordination and tool use inside [[hermes-agent]] — authenticated via ChatGPT Pro OAuth login
rather than an OpenAI API key, avoiding per-token billing.

## Use Cases

- Main reasoning/coordination model driving [[hermes-agent]]'s general agent loop and tool calls,
  as opposed to the narrow coding-specialist role played by [[claude-code]].

## Setup Notes

- Flat-rate access via ChatGPT Pro ($20/mo); heavy use triggers rate limiting rather than added
  cost.
- Avoid connecting Hermes directly to an OpenAI API key, since that starts per-token billing.
- One user reported a ChatGPT weekly rate limit lockout lasting up to 4 days after heavy agent
  tool-call overhead, and was considering OpenRouter with DeepSeek or Gemini 2.5 Flash as a
  flat-rate backup layer (not yet automated as failover).

## Related Concepts

- [[hermes-agent]]
- [[claude-code]]
- [[multi-agent-role-separation]]
