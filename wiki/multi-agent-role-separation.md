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

# Multi-Agent Role Separation (Orchestrator + Specialist Pattern)

## Summary

Instead of forcing one model in one chat window to do everything, split responsibilities across
models and tools by role: an always-on orchestrator, a main reasoning "brain," a narrow-domain
specialist worker, a remote interface, and local execution tools.

## Details

Example stack from the source: [[hermes-agent]] (orchestrator/automation layer) +
[[openai-codex]] (main agent brain) + [[claude-code]] (subscription-based coding specialist) +
Telegram (remote interface) + local machine tools (files, cron, email, shell, home automation).

Workflow: the user states intent to the orchestrator → the orchestrator decides direct handling
vs. delegating to the specialist → the specialist executes → the orchestrator verifies, runs
simple tests, connects to other systems, and reports back.

Reported to work better — and cheaper — than using one large model as the sole "main brain," and
better than running a local LLM (e.g. Ollama) for the full agent loop, which was technically
workable but too slow for general agent use.

One commenter noted that repeated orchestrator-to-specialist review cycles let the orchestrator's
skill system learn routing patterns over time, so the setup gets faster the longer it runs.

## Connections

- [[hermes-agent]]
- [[claude-code]]
- [[openai-codex]]

## Open Questions

- How well this generalizes to backup/failover model routing: one user hit a 4-day ChatGPT weekly
  rate-limit lockout and was considering OpenRouter + DeepSeek/Gemini 2.5 Flash as a flat-rate
  backup layer, but had not automated failover between primary and backup models.
