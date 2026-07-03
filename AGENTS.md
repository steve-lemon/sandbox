# Agent Instructions

This repository is a markdown knowledge vault shared by Hermes, Obsidian, and LLM agents.

Before editing, read:

1. `VAULT_RULES.md`
2. `wiki/VAULT_MEMORY.md`
3. `wiki/INDEX.md`
4. Matching files in `templates/` when creating notes or outputs

Use the Hermes vault skills as the source of truth for operational workflows:

- `projects/second-brain/config/skills/vault-ingest.md`
- `projects/second-brain/config/skills/vault-ingest-claude.md`
- `projects/second-brain/config/skills/vault-query.md`
- `projects/second-brain/config/skills/vault-lint.md`

At runtime, never silently fallback to `~/knowledge`. Use the user-provided `VAULT_DIR`,
or infer the vault root from the current working directory only when the expected vault
structure is present. If the vault root is unclear, ask before editing.

Do not edit file contents in `raw/`. Save durable answers under `outputs/` unless a task is clearly project-scoped.
Use templates before inventing new note structures.

When Obsidian is running and an Obsidian CLI skill is available, prefer it for vault search, backlinks, link analysis, and frontmatter edits. Use direct file tools for bulk edits.
