---
type: concept
topics:
  - ai-coding-tools
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-03"
updated: "2026-07-03"
---

# Claude Subscription vs. API Billing Split for Programmatic Use

## Summary

Anthropic announced that, starting 2026-06-15, programmatic use of Claude Code (`claude -p`, the
Agent SDK, GitHub Actions, and third-party harnesses) is billed separately from the interactive
Claude subscription pool. Interactive Claude Code use in a terminal remains covered by the
subscription.

## Details

- Each subscription tier gets a separate monthly credit pool billed at API rates: Pro $20,
  Max 5x $100, Max 20x $200. Credits do not roll over.
- Interactive Claude Code in a terminal stays on the subscription; programmatic paths (`-p` flag,
  Agent SDK, GitHub Actions, third-party harnesses) move to the new credit pool.
- Known bug noted as of the source's 2026-05-14 edit: for some users, `claude -p` headless mode
  silently falls back to API billing even without `ANTHROPIC_API_KEY` set, instead of using the
  Max subscription. Check `claude /status` and `echo $ANTHROPIC_API_KEY` to confirm which path is
  actually active.
- Separate risk beyond the headless bug: third-party-harness signatures in the request payload
  (e.g. a "HERMES.md" reference in a commit message) can trip Anthropic's backend classifier and
  get billed as third-party-harness/API usage even when the user believes they're on the
  subscription path. One documented case cited a $200 API bill from this.
- Mitigation mentioned by the source: moving toward interactive `tmux` sessions monitored by the
  orchestrator, since interactive terminal use is unaffected by the split.

## Comparisons

Not a model comparison — this is a billing/policy note about how the same [[claude-code]] CLI is
metered depending on invocation path (interactive terminal vs. programmatic/headless).

## Related Concepts

- [[claude-code]]
- [[hermes-agent]]
