# Vault Memory

Running log of vault-ingest activity and standing knowledge about the vault's own state. Read this before any vault operation (per `VAULT_RULES.md`).

## Vault State

- First ingest run for this vault. `wiki/`, `wiki/topics/`, `wiki/INDEX.md`, and this file were created during this run (previously absent).
- Active topics: `ai-agent-orchestration`.

## Ingest Log

### 2026-07-03

- Processed 1 clipping: `드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md` (Reddit r/hermesagent post + comments, translated to Korean in the clipping).
- Created 5 wiki articles: `hermes-agent`, `claude-code`, `openai-codex`, `multi-agent-role-splitting`, `claude-cli-subprocess-delegation`.
- Created 1 topic page: `topics/ai-agent-orchestration`.
- Moved source to `raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md`.
- Flagged `needs-update`: the source's claim that Anthropic split headless/programmatic Claude Code billing (`-p`, Agent SDK, CI) from the interactive subscription pool effective 2026-06-15. Single-source (Reddit), unverified against a primary Anthropic announcement — re-check when a corroborating source appears.
- Flagged `needs-update`: an alleged bug where headless `claude -p` silently bills to the API instead of the Max subscription, and where harness signatures (e.g. a "Hermes" string) can trip usage classifiers. Also single-source and unverified.
