---
type: model
topics:
  - ai-agent-orchestration
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# OpenAI Codex

## Summary

Used in this setup as the "main brain" model behind the [[hermes-agent|Hermes]] orchestrator, handling general back-and-forth conversation and tool use while coding tasks are delegated elsewhere.

## Capabilities

- General agentic back-and-forth and tool use as the primary model slot for [[hermes-agent|Hermes]]

## Constraints

- Accessed via ChatGPT Pro OAuth login rather than an API key, so heavy use triggers rate limiting rather than per-token cost; the source reports being locked out for up to 4 days after hitting a weekly limit.

## Comparisons

- Preferred over running a local LLM (the source tried Ollama locally): technically functional but "too slow" for full agentic use on underpowered hardware.

## Related Concepts

- [[hermes-agent|Hermes]]
- [[multi-agent-role-splitting]]
