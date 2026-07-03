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

# Hermes

## Summary

Always-on orchestrator agent that acts as the coordination layer in a multi-agent AI setup. Holds memory, tool access, scheduled jobs (cron), and messaging, and decides whether to handle a request directly or delegate coding work to a specialist like [[claude-code|Claude Code]].

## Use Cases

- Coordinating a multi-model workflow instead of forcing one model to do everything ([[multi-agent-role-splitting]])
- Delegating focused coding tasks to [[claude-code|Claude Code]] via CLI subprocess ([[claude-cli-subprocess-delegation]])
- Using [[openai-codex|OpenAI Codex]] as the main "brain" for general back-and-forth and tool use
- Acting as a hub for Telegram-based remote control, email, shell access, cron, and Home Assistant integration

## Setup Notes

- Runs via OAuth/subscription logins rather than API keys to avoid per-token billing: OpenAI Codex through a ChatGPT Pro subscription, Claude Code through a Claude Max subscription.
- Calls the `claude` CLI directly via subprocess (`claude -p "task" --max-turns 10`); no wrapper needed. See [[claude-cli-subprocess-delegation]].
- Setup snag: Hermes-managed Node installs the `claude` binary under `~/.hermes/node/bin/claude`, which is not on `PATH` by default — the source symlinked it into `~/.local/bin/`.
- For longer sessions, one user ran Claude interactively inside `tmux` with Hermes monitoring, instead of headless `-p` calls.

## Related Concepts

- [[multi-agent-role-splitting]]
- [[claude-code|Claude Code]]
- [[openai-codex|OpenAI Codex]]
- [[claude-cli-subprocess-delegation]]

## Open Questions

- needs-update: no confirmed backup/failover model when the ChatGPT Pro rate limit is hit. Source only speculates about OpenRouter + DeepSeek or Gemini 2.5 Flash as an untested fallback.
