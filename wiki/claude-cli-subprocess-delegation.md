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

# Claude CLI Subprocess Delegation

## Summary

An orchestrator delegates coding work to [[claude-code|Claude Code]] by shelling out to the `claude` CLI as a plain subprocess, rather than calling the Anthropic API directly — keeping usage on a Claude subscription instead of per-token billing.

## Details

- Call pattern: `claude -p "<task>" --max-turns 10`, invoked directly from the orchestrator's shell-out capability (no custom wrapper).
- Because Claude Code authenticates itself via its own OAuth against a Claude Max subscription, the orchestrator never touches the Anthropic API — from Anthropic's side it looks identical to a human typing `claude -p "task"` in a terminal.
- What this does *not* permit: using a Claude subscription as a model provider that the orchestrator calls directly over the API. The distinction is CLI-subprocess (allowed under the subscription) vs. direct API use as a model backend (not covered by the subscription).
- Practical setup snag: Hermes-managed Node drops the `claude` binary at `~/.hermes/node/bin/claude`, off the default `PATH` — resolved by symlinking it into `~/.local/bin/`.
- Alternative for long sessions: run Claude interactively inside `tmux` while the orchestrator monitors, instead of headless `-p` calls.

## Connections

- [[hermes-agent|Hermes]]
- [[claude-code|Claude Code]]
- [[multi-agent-role-splitting]]

## Open Questions

- needs-update: source claims (2026-05-14 edit, unverified) that Anthropic separated programmatic Claude Code usage (`-p`, Agent SDK, CI/third-party harnesses) from the interactive-terminal subscription pool effective 2026-06-15, onto separate monthly API-billed credits (Pro $20 / Max 5x $100 / Max 20x $200, non-rollover). If accurate, this pattern's "subscription, not API billing" claim would no longer hold for headless `-p` calls after that date.
- needs-update: source also claims a bug where headless `-p` mode can silently bill to the API even without an API key set, and that harness signatures in project files (e.g., a literal "Hermes" string) can trip usage classifiers into API billing. Single unverified source (Reddit thread) — worth confirming independently before relying on this pattern for cost control.
