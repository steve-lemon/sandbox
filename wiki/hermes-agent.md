---
type: framework
topics:
  - ai-agent-orchestration
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# Hermes (AI Agent Orchestrator)

## Summary

Always-on orchestrator/coordinator agent. It holds memory, tools, scheduled jobs (cron), and
messaging (e.g. Telegram), and can act directly on the user's machine — sending email, running
scripts, checking files, and managing cron. In a multi-agent setup it decides whether to handle a
request itself or delegate coding work to a specialist like [[claude-code]].

## Principles

- Don't force one model in one chat window to do everything — see [[multi-agent-role-separation]].
- Runs well on a small always-on Linux box, mini PC, NUC, or server.

## Workflow

1. The user tells Hermes what they want.
2. Hermes decides whether to handle it directly or delegate the coding portion to
   [[claude-code]].
3. Claude Code writes, reviews, or modifies the code.
4. Hermes checks the results, runs simple tests, connects to other systems, and reports back.

## When To Use

As the orchestration/automation layer in a personal multi-agent setup, coordinating a main
reasoning model ([[openai-codex]]) and a coding specialist ([[claude-code]]) rather than acting as
the sole model.

## Related Concepts

- [[multi-agent-role-separation]]
- [[claude-code]]
- [[openai-codex]]
- [[claude-code-subscription-billing-split]] — a documented $200 API bill occurred when a commit
  message referenced "HERMES.md," which Anthropic's backend flagged as third-party-harness usage.
- Known (fixed) Hermes bug: pairing Telegram with the OpenAI Codex provider without an API key set
  could produce a `'NoneType' object is not iterable` error that halted the agent.
