# 이 vault 기준, Hermes와 Obsidian의 역할 차이를 정리해

## Answer

이 vault의 문서 기준으로 Hermes와 Obsidian은 층위가 다른 역할을 맡는다.

**Hermes — 실행/오케스트레이션 계층 (능동적 에이전트)**

- 상시 실행(always-on)되는 오케스트레이터 에이전트로, 메모리·도구·예약 작업(cron)·메시징을 보유한다.
- 실제로 로컬 컴퓨터에서 작업을 "수행"한다 — 이메일 전송, 스크립트 실행, 파일 확인, 쉘 명령, Home Assistant 연동 등.
- 코딩 작업은 직접 하지 않고 [[claude-code-subprocess-delegation|Claude Code에 서브프로세스로 위임]]하며, 별도의 "메인 두뇌" 모델(OpenAI Codex 등)과 결합해 일반적인 조정·도구 사용을 처리한다.
- Telegram 같은 원격 인터페이스로 어디서든 시스템에 접근하게 해준다.
- 요약하면 Hermes는 "관리자(orchestrator)" — 판단하고, 위임하고, 도구를 실행하는 행위자다.

**Obsidian — 지식 저장/그래프 계층 (수동적 보관소 + 조회 도구)**

- `VAULT_RULES.md`에 따르면 이 vault 자체가 "Hermess와 Obsidian을 통해 운영되는 AI Second Brain"으로 정의되어 있다. 즉 Obsidian은 지식(노트)이 실제로 저장되고 서로 연결되는 vault 저장소/뷰어 역할을 한다.
- `wiki/` 아티클의 frontmatter, `[[wikilink]]`, `[[note-slug|Alias]]` 같은 표기 규칙은 모두 Obsidian의 노트 연결 문법을 그대로 따른다 — 즉 지식 그래프의 구조를 규정하는 쪽이 Obsidian이다.
- Obsidian이 실행 중일 때는 `/obsidian-cli` 스킬로 검색(`search:context`), 백링크 확인(`backlinks`), frontmatter 편집 같은 "조회/탐색" 작업을 수행한다 — 대량 편집은 여전히 직접 파일 도구(Read/Edit/Write)를 쓴다.
- 요약하면 Obsidian은 "저장소/그래프 뷰어" — 능동적으로 작업을 수행하지 않고, 지식이 어떻게 구조화·연결·조회되는지를 담당한다.

**한 줄 대비**: Hermes는 "무엇을 할지 결정하고 실행하는" 능동적 오케스트레이터, Obsidian은 "지식이 어떻게 저장·연결·조회되는지"를 규정하는 수동적 지식 그래프 계층이다. 둘은 경쟁 관계가 아니라, Hermes가 vault 밖에서 행동을 조정하고 Obsidian이 vault 안에서 그 결과(wiki 문서)를 구조화·보존하는 상보적 관계다.

## Evidence

- [[hermes-agent]] — Hermes의 오케스트레이터 역할, 로컬 도구/스케줄링/메시징, Claude Code 위임 구조
- `VAULT_RULES.md` (vault 시스템 문서, wiki 아티클 아님) — "AI Second Brain operated through Hermess and Obsidian" 정의, Obsidian CLI 사용 규칙, wikilink/frontmatter 규약
- `AGENTS.md` (vault 시스템 문서) — 동일한 Hermess+Obsidian 운영 구조 재확인

## Notes

- 이 vault의 `wiki/` 아티클 중 Obsidian 자체를 다루는 전용 문서(`type: tool`)는 아직 없다. 위 Obsidian 관련 설명은 `wiki/` 근거가 아니라 `VAULT_RULES.md`/`AGENTS.md` 같은 vault 시스템 문서에서 가져온 것이며, 이는 vault-query 규칙상 "vault 안의 근거"로 인정되지만 wiki 아티클 수준의 검증(concept/status 태깅)은 거치지 않은 상태다.
- 필요하다면 `wiki/obsidian.md` (type: tool) 문서를 신규 생성해 이 정보를 정식 wiki 개념으로 승격하는 것을 권장한다 (현재는 추가 확인/생성 필요 상태).
