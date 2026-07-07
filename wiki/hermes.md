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

# Hermes

## Summary

Hermes is described as an always-on personal AI agent that acts as the coordinator/orchestrator in a multi-tool agent stack, rather than as the main reasoning model itself. It holds memory, tools, scheduled jobs, and messaging, and can act directly on a local machine.

## Use Cases

- Central coordinator that decides whether to handle a user request directly or delegate coding work to a specialist tool (see [[claude-code|Claude Code]]).
- Runs scheduled/cron jobs, sends email, executes scripts, checks files, and manages Home Assistant-style local integrations.
- Bridges a remote chat interface (e.g. Telegram) to local machine actions.
- Can shell out to another CLI (e.g. `claude -p "task" --max-turns 10`) as a subprocess, reads the result, verifies changes, and reports back.

## Setup Notes

- One reported setup runs Hermes via OpenAI OAuth login (ChatGPT Pro subscription) to avoid per-token API billing for the orchestrator role itself; see [[openai-codex|OpenAI Codex]] for the main-brain role filled separately.
- A Hermes-managed Node install can place the `claude` binary at `~/.hermes/node/bin/claude`, which is not on `PATH` by default — one user worked around this by adding it to `.bashrc` and symlinking into `~/.local/bin/`.
- needs-update / unverified single anecdote: one Reddit commenter reported an approximately $200 Anthropic API charge after a project file (e.g. a committed `HERMES.md`) let Anthropic's backend detect third-party-harness usage and route calls to metered API billing instead of the Claude subscription pool. Not confirmed independently; treat as a caution, not a documented mechanism.
- This vault's own `vault-ingest-claude` workflow independently uses "Hermes" as the name for the delegating/fallback orchestrator role (see `projects/second-brain/config/skills/vault-ingest-claude.md`) — confirm before assuming it is the same product as described in this source.

## Related Concepts

- [[multi-agent-orchestration]]
- [[claude-code]]
- [[openai-codex]]

## Open Questions

- Whether "Hermes" here is the same software referenced elsewhere in this vault's own skill files, or a separate product with the same name — needs-update.
