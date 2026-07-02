---
type: query-output
created: "2026-07-03"
question: "이 vault 기준! Hermes와 Obsidian의 역할 차이를 정리해"
sources:
  - "VAULT_RULES.md"
  - "AGENTS.md"
  - "wiki/hermes-agent.md"
---

# 이 vault에서 Hermes와 Obsidian의 역할 차이

## Answer

이 vault는 `VAULT_RULES.md` 1행에 정의된 대로 **"Hermess와 Obsidian이 함께 운영하는 AI Second
Brain"**이다. 둘은 경쟁 관계가 아니라 **실행 엔진 vs. 탐색/편집 UI**로 역할이 나뉜다.

**Hermes — 자동화/실행 엔진**
- 상시 실행되는 오케스트레이터로, 메모리·도구·cron·메시징(Telegram)을 갖고 실제로 파일을
  읽고 쓴다 ([[hermes-agent]]).
- 이 vault 맥락에서는 `vault-ingest` / `vault-query` / `vault-lint` 같은 Hermess 스킬을 실행해
  `Clippings/` → `raw/` + `wiki/` 컴파일, wiki 기반 질의응답, 품질 점검을 수행하는 주체다.
- 코딩처럼 좁고 깊은 작업은 Claude Code 같은 전문 워커에게 위임할지 스스로 판단한다
  ([[multi-agent-role-separation]]).
- 즉, vault 안에서 "누가 파일을 만들고 고치는가"에 해당하는 능동적 실행자다.

**Obsidian — 지식 탐색/편집 UI**
- vault의 실체는 `wiki/`, `raw/`, `outputs/`, `templates/`, `projects/`, `docs/`로 구성된 순수
  마크다운 파일 구조이고, Obsidian은 이 구조를 사람이 그래프 뷰·백링크·`[[wikilink]]`·
  frontmatter로 탐색하고 편집할 수 있게 해주는 클라이언트다.
- `VAULT_RULES.md`는 "Obsidian이 실행 중일 때는 `/obsidian-cli` 스킬(검색, 링크 분석,
  frontmatter 편집)을 grep보다 우선 사용하라"고 명시한다 — 즉 Obsidian은 조회·탐색·소규모
  편집을 위한 인터페이스 계층이지, Hermes처럼 스킬을 실행하거나 clippings를 자동 처리하는
  자동화 엔진이 아니다.
- 대량 작업(예: 여러 파일에 걸친 일괄 수정)에는 Obsidian UI 대신 직접 파일 도구를 쓰라고
  명시되어 있다 — Obsidian은 어디까지나 "가벼운 상호작용" 계층으로 취급된다.

**요약 대비**

| | Hermes | Obsidian |
| --- | --- | --- |
| 성격 | 자동화/실행 엔진 (능동적) | 지식 그래프 뷰어/에디터 (사람 인터페이스) |
| 주요 동작 | 스킬 실행, 파일 생성/수정, 스케줄링, 메시징 | 검색, 백링크 확인, 프론트매터 편집, 그래프 탐색 |
| vault 내 위치 | vault 밖에서 vault를 조작하는 에이전트 | vault(마크다운 파일)를 보여주는 뷰어 |
| 이 vault의 활용 예 | `vault-ingest`/`vault-query`/`vault-lint` 스킬 실행 | `/obsidian-cli search`, `backlinks` 명령 |

## Evidence

- [[hermes-agent]] — Hermes를 "상시 실행 오케스트레이터, 메모리·도구·cron·메시징 보유,
  코딩 위임 여부 판단"으로 설명 (단, 이 문서는 외부 Reddit 클리핑에서 온 일반적인 Hermes
  운영 사례이며 이 vault 전용 설명은 아님).
- `VAULT_RULES.md` — 이 vault 자체가 "Hermess + Obsidian"으로 운영됨을 명시하고,
  Obsidian CLI 스킬의 우선 사용 범위(검색/백링크/frontmatter)와 한계(대량 작업은 직접 파일
  도구 사용)를 규정.
- `AGENTS.md` — 에이전트(=Hermes/LLM)가 vault 편집 전에 읽어야 할 문서와 워크플로우를 정의,
  Obsidian은 별도로 언급되지 않고 vault 구조 자체로만 전제됨.

## Notes

- 이 vault에는 아직 **Obsidian 자체를 다루는 전용 wiki 문서가 없다.** 위 Obsidian 관련 설명은
  wiki 노트가 아니라 `VAULT_RULES.md`(이 vault의 운영 규칙 문서)에 근거한 것이며, 별도
  클리핑/소스로 뒷받침되지는 않는다 — 필요하면 `wiki/obsidian.md` (type: tool)를 만들어
  근거를 보강할 수 있다.
- [[hermes-agent]]의 설명은 이 vault를 위해 쓰인 게 아니라 외부 클리핑(Reddit)에서 온
  일반적인 Hermes 운영 사례이므로, 이 vault의 Hermess 스킬 시스템과 세부 구현이 100% 같다는
  보장은 없다 — 개념적 유사성에 근거한 추론이다.

## Follow-ups

- `wiki/obsidian.md` 신규 작성 여부 검토 (Obsidian의 role을 독립 개념 노트로 승격할지)
- 이 vault의 Hermess 스킬 시스템 자체를 설명하는 wiki 노트(`hermess-vault-system` 등)를
  만들어 [[hermes-agent]]와 구분할지 검토
