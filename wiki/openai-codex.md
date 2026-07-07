---
type: tool
topics:
  - ai-agent-tooling
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# OpenAI Codex

## Summary

In the source setup, OpenAI Codex fills the "main agent brain" role inside a multi-agent stack: general back-and-forth reasoning and tool use, coordinated by [[hermes|Hermes]] and separate from the dedicated coding specialist ([[claude-code|Claude Code]]).

## Use Cases

- Main reasoning/tool-use model for an always-on personal agent, handling everything that isn't routed to the coding specialist.

## Setup Notes

- Reported as accessed through a ChatGPT Pro subscription via OAuth login rather than a metered API key, to avoid per-token billing; heavy use was reported to trigger rate limiting rather than additional charges.
- One commenter reported combining a ChatGPT Pro ($20) subscription for Codex-as-orchestrator with OpenRouter-hosted models (e.g. DeepSeek, Gemini) as cheaper delegated workers when routing logic sends tasks their way.

## Related Concepts

- [[multi-agent-orchestration]]
- [[hermes]]

## Open Questions

- needs-update: no detail in the source on how Codex's tool-calling is wired into Hermes beyond "handles the main agent side well" — mechanism unconfirmed.
