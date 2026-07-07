---
type: concept
topics:
  - ai-agent-tooling
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# Multi-Agent Orchestration (Role Specialization)

## Summary

A pattern for personal AI agent setups where separate models/tools are assigned distinct roles instead of forcing one model to do everything in a single chat window. One agent acts as an always-on orchestrator, a second acts as the main reasoning "brain", and a third is called out as a specialized worker for a narrow task class (e.g. coding).

## Details

Reported stack (from a single user's setup, [[hermes|Hermes]] + [[openai-codex|OpenAI Codex]] + [[claude-code|Claude Code]]):

- **Orchestrator** — always-on agent holding memory, tools, scheduled jobs (cron), and messaging; it decides whether to handle a request directly or delegate it.
- **Main agent brain** — handles general back-and-forth reasoning and tool use.
- **Specialist worker** — invoked only for a bounded task class (coding), then its output is reviewed and reported back by the orchestrator.
- **Remote interface** — a chat surface (e.g. Telegram) for reaching the system from anywhere.
- **Local execution environment** — an always-on machine (small Linux box, mini PC, NUC) running files, cron, email, shell, and home-automation tools.

Delegation is done by shelling out to a CLI subprocess rather than building a custom API wrapper, e.g. `claude -p "task" --max-turns 10`. Because the coding tool authenticates with its own subscription OAuth session, the orchestrator never touches that provider's API directly — from the provider's side it looks identical to a human typing the command in a terminal.

Cited motivation for this pattern: running a single local/small model for every role (the author tried Ollama locally) was reported as too slow for full agentic use; running everything through one hosted model's chat window was reported as worse than splitting roles by specialty ("the single-model trap").

## Connections

- [[hermes]]
- [[openai-codex]]
- [[claude-code]]

## Open Questions

- needs-update: whether provider billing changes (see [[claude-code#Setup Notes]]) alter the economics of the CLI subprocess delegation pattern described here.
- No independent verification of the "single model is worse" claim beyond one user's anecdote — treat as an inference, not an established result.
