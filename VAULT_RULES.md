# Vault Rules

## Session Start

Before any vault operation, read:

1. `wiki/VAULT_MEMORY.md`
2. `wiki/INDEX.md`
3. The relevant Hermess skill instructions

## Directory Contract

| Directory | Role |
| --- | --- |
| `Clippings/` | New source inbox |
| `raw/` | Processed source originals. Append-only |
| `wiki/` | Concept articles |
| `wiki/topics/` | Topic index pages |
| `outputs/` | Query answers and analysis reports |
| `projects/` | Project execution context |
| `docs/` | System and configuration docs |

## Core Rules

- Do not edit files in `raw/`.
- Preserve source provenance.
- Create one wiki article per concept.
- Use wikilinks for related concepts.
- Save retained answers under `outputs/`.
- Prefer updating existing notes over creating duplicates.
