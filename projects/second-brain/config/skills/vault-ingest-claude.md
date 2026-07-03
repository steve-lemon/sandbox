---
name: vault-ingest-claude
description: >
  사용자의 knowledge vault($VAULT_DIR)의 Clippings 처리를
  Hermes가 직접 수행하지 않고 Claude CLI/Claude Code에 위임한다. Hermes는
  트리거, 동시 실행 방지, 결과 검증, 요약 보고를 담당한다.
---

# Vault Ingest Claude (Hermes -> Claude)

이 스킬은 split/hybrid 구성용이다.

- Hermes: 트리거, `VAULT_DIR` 확인, Clippings 존재 확인, lock, Claude 호출, 결과 검증, 보고
- Claude: 원문 읽기, 개념 추출, wiki 작성, templates 적용, raw 이동, index/topic/memory 갱신

`vault-query`와 `vault-lint`는 Hermes-native로 유지하고, ingest만 Claude에 위임하고 싶을 때 사용한다.

## 언제 사용하는가

- 사용자가 "클리핑 처리해줘"라고 요청했고 ingest 품질을 Claude에 맡기고 싶을 때
- 긴 원문, 복잡한 개념 분해, 다수 wiki 문서 생성이 필요한 경우
- Hermes의 현재 모델은 GPT/Codex이지만, 자료 컴파일은 Claude Code가 더 적합하다고 판단될 때

## 절차

런타임에서는 `~/knowledge`로 조용히 fallback하지 않는다.

`VAULT_DIR` 결정 순서:

1. 사용자가 `VAULT_DIR`를 명시하면 그 값을 우선한다.
2. 명시되지 않았고 현재 작업 루트가 Obsidian vault 구조(`VAULT_RULES.md`, `wiki/`,
   `raw/`, `Clippings/`, `templates/`)를 갖고 있으면 현재 작업 루트를 `VAULT_DIR`로
   간주한다.
3. 확신할 수 없으면 Claude를 호출하지 말고 사용자에게 vault 경로를 확인한다.

Claude 호출 전 `VAULT_DIR`는 반드시 절대경로로 resolve한다. 예를 들어 테스트 vault가
`~/sandbox`라면 `ABSOLUTE_VAULT_DIR`는 `/.../sandbox`여야 한다. `~/knowledge`는
설치 예시일 뿐이며, `ABSOLUTE_VAULT_DIR`가 실제로 그 경로일 때만 사용한다.

1. `$VAULT_DIR/Clippings/`에 처리 대기 파일이 있는지 확인한다.
   비어 있으면 Claude를 호출하지 않고 "처리할 클리핑 없음"으로 종료한다.
2. `$VAULT_DIR/.locks/`가 없으면 생성하고, `$VAULT_DIR/.locks/vault-ingest.lock`으로
   동시 실행을 방지한다. 이미 lock이 있으면 실행하지 말고 사용자에게 보고한다.
3. resolve된 `$ABSOLUTE_VAULT_DIR` 루트에서 Claude CLI를 호출한다.
4. Claude에는 아래 job spec을 그대로 전달한다.
5. Claude 실행이 끝나면 다음을 검증한다.
   - Claude가 보고한 작업 경로가 `$ABSOLUTE_VAULT_DIR`와 일치하는지
   - 생성/수정/이동된 모든 파일 경로가 `$ABSOLUTE_VAULT_DIR` 아래인지
   - `Clippings/`가 비었는지
   - 처리 원문이 `raw/`로 이동했는지
   - 새/수정 wiki 문서가 `templates/` 구조와 frontmatter를 따르는지
   - `sources`가 `"raw/<source-file-name>.md"` 형식인지
   - Obsidian alias가 `[[note-slug|Alias]]` 형식인지
   - `wiki/INDEX.md`, `wiki/topics/`, `wiki/VAULT_MEMORY.md`가 갱신됐는지
6. lock을 제거한다.
7. 사용자에게 처리 파일, 생성/수정 문서, 검증 결과, 남은 이슈를 요약한다.

## Claude job spec

```text
You are processing an Obsidian markdown knowledge vault.

ABSOLUTE_VAULT_DIR: <resolved absolute vault path>

Before editing:
- Confirm that the current working directory is ABSOLUTE_VAULT_DIR.
- If the current working directory is not ABSOLUTE_VAULT_DIR, stop and report the mismatch.
- Only read or write files under ABSOLUTE_VAULT_DIR.
- Do not use ~/knowledge unless ABSOLUTE_VAULT_DIR is exactly the resolved ~/knowledge path.

Read first:
- VAULT_RULES.md
- CLAUDE.md, if present
- wiki/VAULT_MEMORY.md
- wiki/INDEX.md
- templates/
- projects/second-brain/config/skills/vault-ingest.md

Task:
Process every markdown file in Clippings/.
Move processed originals to raw/ without changing their content.
Create or update wiki notes using matching templates in templates/.
Prefer updating existing wiki notes over creating duplicate notes.
Update wiki/topics/, wiki/INDEX.md, and wiki/VAULT_MEMORY.md.

Rules:
- Do not edit raw file contents.
- Record raw source provenance as "raw/<source-file-name>.md", not as a raw-file wikilink.
- Use Obsidian aliases as [[note-slug|Alias]], not [[note-slug\|Alias]].
- Mark unsupported or time-sensitive claims as needs-update or TODO.
- Do not run git push, git reset, git clean, rm -rf, or destructive commands.

Final response:
Report the current working directory, ABSOLUTE_VAULT_DIR, processed clipping files,
created wiki notes, updated wiki notes, new stubs, topic/index/memory updates,
and any unresolved issues.
```

## Claude CLI 호출 예시

실제 환경에서 `claude`가 PATH에 없다면 절대경로를 사용한다.

```bash
ABSOLUTE_VAULT_DIR="$(cd "${VAULT_DIR:-$PWD}" && pwd)"
cd "$ABSOLUTE_VAULT_DIR" && claude -p "<CLAUDE_JOB_SPEC with ABSOLUTE_VAULT_DIR=$ABSOLUTE_VAULT_DIR>" --permission-mode acceptEdits --output-format json
```

## 금지 사항

- `Clippings/`가 비어 있는데 Claude를 호출하지 않는다
- lock이 있는데 병렬 실행하지 않는다
- Claude 실행 실패 시 자동 재시도를 반복하지 않는다
- Hermes가 Claude 결과 검증 없이 성공으로 보고하지 않는다
