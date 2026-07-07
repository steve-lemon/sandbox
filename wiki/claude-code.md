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

# Claude Code

## Summary

Claude Code is used in the source as a focused coding specialist inside a larger multi-agent stack, invoked from an orchestrator ([[hermes|Hermes]]) as a CLI subprocess rather than through the Anthropic API directly, so that usage counts against a Claude subscription plan instead of metered API billing.

## Use Cases

- Delegated, bounded coding tasks: write, review, or fix code, then report results back to the orchestrator.
- Invoked non-interactively via `claude -p "task" --max-turns 10`, or interactively inside a `tmux` session that the orchestrator monitors for longer sessions.

## Setup Notes

- Reported to authenticate via its own OAuth session against a Claude Max/Pro subscription, so the orchestrator process never calls the Anthropic API directly — from Anthropic's side this is indistinguishable from a human running `claude` in a terminal, per the source.
- What is *not* supported per the source: using a Claude subscription as a model provider that another orchestrator calls directly over the Anthropic API. Shelling out to the `claude` CLI as a subprocess is reported to be fine; direct API-as-provider use is not.
- needs-update (time-sensitive, confirm current terms): the source's 2026-05-14 edit states Anthropic announced that, starting 2026-06-15, `claude -p` and Agent SDK usage would be split out from the interactive Claude Code subscription pool into separate metered monthly credit buckets billed at API rates (cited figures: Pro $20, Max 5x $100, Max 20x $200), non-rolling, while interactive terminal use of Claude Code stays on the subscription. As of this vault's current date, that effective date has already passed — verify against current Anthropic documentation rather than relying on this secondhand summary.
- needs-update / unverified: source also describes a reported bug where headless `claude -p` mode could silently bill to the API even without `ANTHROPIC_API_KEY` set, and separately that a third-party harness signature detected in project files (e.g. a `HERMES.md` reference) could cause calls to be classified as third-party-harness usage and billed via the API instead of the subscription. Suggested checks from the source: run `claude /status`, check `echo $ANTHROPIC_API_KEY`, and check the provider's billing dashboard for unexpected API usage.

## Related Concepts

- [[multi-agent-orchestration]]
- [[hermes]]
- [[openai-codex]]

## Open Questions

- needs-update: confirm current (post 2026-06-15) Claude Code billing behavior directly against Anthropic's documentation; this note reflects one secondhand Reddit summary only.
