---
type: pattern
topics:
  - ai-agent-orchestration
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# Multi-Agent Role Splitting

## Summary

Instead of forcing one model to act as orchestrator, reasoning engine, and coding specialist all at once, assign each role to the tool best suited for it and let a lightweight orchestrator route work between them.

## Details

Example split from the source:

- [[hermes-agent|Hermes]] — always-on orchestrator: memory, tools, cron, messaging, task coordination
- [[openai-codex|OpenAI Codex]] — main agent "brain" for general reasoning and tool use
- [[claude-code|Claude Code]] — dedicated coding specialist, invoked only for coding work
- Telegram — remote-control interface
- Local machine — execution environment (files, cron, email, shell, home automation)

Workflow: user tells Hermes what they want → Hermes decides whether to handle it directly or delegate the coding portion to Claude Code → Claude Code writes/reviews/modifies code → Hermes verifies the result, runs basic checks, and reports back.

Motivation cited: running everything through a single local/small model (the source tried Ollama) was reliable but too slow for full agentic use; splitting roles across existing subscriptions (ChatGPT Pro + Claude Max) avoided both per-token API costs and single-model bottlenecks.

## Connections

- [[hermes-agent|Hermes]]
- [[claude-code|Claude Code]]
- [[openai-codex|OpenAI Codex]]
- [[claude-cli-subprocess-delegation]]

## Open Questions

- needs-update: one commenter mentions a separate, more elaborate multi-model pipeline (Opus 4.7 planning → GPT-5.5 implementing → Sonnet 4.6 reviewing → GPT-5.5 QA) combined with TDD + "JiT" + vertical-slice principles, but gives no further detail. Noted here as a related but unexplored variant, not confirmed or explained by the source.
