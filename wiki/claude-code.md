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

# Claude Code (as a Delegated Coding Worker)

## Summary

In this setup, Claude Code is used as a focused coding worker invoked as a subprocess from an
orchestrator ([[hermes-agent]]), authenticated through the user's Claude Max subscription (OAuth)
rather than the Anthropic API — so delegated coding work does not consume separate API token
credits.

## Use Cases

- Delegated "write, review, or modify code" tasks issued by [[hermes-agent]].
- Headless invocation: `claude -p "task here" --max-turns 10`.
- Interactive invocation inside a `tmux` session for longer sessions, with the orchestrator
  monitoring the session.

## Setup Notes

- The Node install managed by Hermes places the `claude` binary at `~/.hermes/node/bin/claude`,
  which is not on `PATH` by default — add it via `bashrc` and symlink into `~/.local/bin/` so
  Hermes can find it.
- Anthropic reportedly sees this the same as the user typing `claude -p "task"` directly in a
  terminal, as long as Hermes shells out to the `claude` CLI rather than calling the Anthropic API
  with the subscription configured as a model provider.
- Not allowed: using the Claude subscription as an API-key model provider inside Hermes.
  Fine: running the `claude` CLI itself as a subprocess.
- Risk: third-party-harness signatures in the payload (e.g. a "HERMES.md" reference) can trip
  Anthropic's backend classifier and get billed to the API even under a subscription. See
  [[claude-code-subscription-billing-split]].

## Related Concepts

- [[hermes-agent]]
- [[multi-agent-role-separation]]
- [[claude-code-subscription-billing-split]]
