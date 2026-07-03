---
type: tool
topics:
  - ai-agent-orchestration
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# Claude Code

## Summary

Anthropic's coding-agent CLI. In this setup it plays a "coding specialist" role: an orchestrator ([[hermes-agent|Hermes]]) shells out to it for focused coding tasks so a Claude subscription can be reused without paying separate per-token API costs.

## Use Cases

- Writing, reviewing, or modifying code on behalf of an orchestrating agent
- Headless delegation: `claude -p "task" --max-turns 10` called as a subprocess from [[hermes-agent|Hermes]]
- Interactive long-running sessions inside `tmux`, monitored by the orchestrator, as an alternative to headless calls

## Setup Notes

- Authenticates with a personal Claude Max subscription via its own OAuth flow; the orchestrator never talks to the Anthropic API directly, so — per the source — it looks like a normal terminal `claude -p` invocation from Anthropic's side.
- needs-update: the source reports (as of a 2026-05-14 edit) that Anthropic announced `claude -p` and Agent SDK usage would move off the Claude subscription pool onto separate monthly API-billed credits starting 2026-06-15, while interactive terminal Claude Code stays on the subscription. That cutover date has now passed (today: 2026-07-03) but this has not been verified against a primary Anthropic source — treat the billing mechanics above as potentially stale.
- needs-update: the source also claims a known bug where headless `claude -p` mode can silently bill to the API instead of the Max subscription even without `ANTHROPIC_API_KEY` set, and that harness signatures (e.g. a "Hermes" string reaching Claude Code) can trip usage classifiers and cause unexpected API billing. Unverified beyond this single Reddit thread — confirm via `claude /status` and provider billing dashboards before relying on it.

## Related Concepts

- [[hermes-agent|Hermes]]
- [[claude-cli-subprocess-delegation]]

## Open Questions

- Whether the 2026-06-15 billing separation actually took effect as described, and what it means for headless orchestration patterns like this one.
