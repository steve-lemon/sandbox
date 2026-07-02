---
type: concept
topics:
  - ai-agents
status: needs-update
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
---

# Anthropic 프로그래매틱 사용 청구 정책 분리 (원문 커뮤니티 보고 기준)

> 이 문서는 Anthropic 공식 발표 원문이 아니라, Reddit 게시물 작성자(kenduffy)가 2026-05-14 편집(edit)에서 요약 인용한 내용에 기반한다. Anthropic 공식 문서로 교차 검증되지 않았으므로 `needs-update`로 표시한다.

## 보고된 내용

게시물 편집분에 따르면, Anthropic이 2026-06-15부터 `claude -p` 및 Agent SDK를 통한 사용을 기존 Claude 구독(Pro/Max) 풀에서 분리한다고 발표했다고 한다.

- 각 구독 티어(Pro $20, Max 5x $100, Max 20x $200)는 API 요금으로 청구되는 별도의 월별 크레딧을 받으며, 이는 이월되지 않음
- **유지되는 것**: 터미널에서의 대화형(interactive) `claude` 사용 — 구독 풀에 남음
- **분리되는 것**: 프로그래매틱 사용 전체 — `-p` 플래그, Agent SDK, GitHub Actions, 서드파티 하네스(예: [[hermes-agent|Hermes]]) 경유 호출

## 관련 우회/버그 리스크

이 정책 변경과 맞물려 [[claude-code-subprocess-delegation|Claude Code 서브프로세스 위임 패턴]]에서 두 가지 "빌링 이스케이프 해치"가 보고됨:

1. `-p` 헤드리스 모드가 API 키 미설정 상태에서도 API 과금으로 자동 전환되는 버그
2. 페이로드 내 서드파티 하네스 시그니처(예: "Hermes" 문자열)가 Anthropic의 분류기에 감지되어 API 사용으로 재분류되는 사례

## 시사점 (원문 저자의 대응 계획)

- 원문 저자는 6/15 이후 해당 `claude -p` 패턴이 "여전히 작동하지만 만료일이 있다"고 언급
- 가능하면 인터랙티브 `tmux` 세션 기반으로 설정을 재작업하겠다고 밝힘 — 이후 결과는 후속 게시물에서 확인 예정이라고 함 (원문 시점 기준 후속 게시물 존재 여부 미확인)

## 검증 필요 사항 (TODO)

- Anthropic 공식 발표/문서 링크로 교차 확인
- 2026-06-15 정책이 실제 시행되었는지, 세부 조건(가격/크레딧 규모)이 원문과 일치하는지 확인
- `claude-api` 관련 최신 정보는 별도 확인 필요 (이 vault 시스템의 `claude-api` 참고 스킬/문서와 대조 권장)

## 관련 문서

- [[claude-code-subprocess-delegation]]
- [[hermes-agent]]
