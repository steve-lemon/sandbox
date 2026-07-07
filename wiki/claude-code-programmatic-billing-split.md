---
type: concept
topics:
  - ai-agents
status: needs-update
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# Claude Code Programmatic Usage Billing Split

## Summary

Anthropic이 2026년 6월 15일부터 [[claude-code|Claude Code]]의 프로그래매틱 사용(`claude -p` 헤드리스 모드, Agent SDK, GitHub Actions, 서드파티 하네스)을 Claude 구독의 정액 사용량 풀에서 분리하고, 티어별 별도 월간 API 크레딧(Pro $20, Max 5x $100, Max 20x $200)으로 청구하기로 발표했다는 내용(원 클리핑 편집일 2026-05-14 기준 예고 공지). 이월은 되지 않음. 터미널에서 사람이 직접 대화형으로 쓰는 Claude Code는 구독에 그대로 남는다고 언급됨.

## Details

- 영향받는 사용 형태: `claude -p`, Agent SDK, GitHub Actions, 서드파티 오케스트레이션 하네스(예: [[hermes-agent|Hermes]]가 서브프로세스로 `claude` CLI를 호출하는 패턴).
- 영향받지 않는다고 언급된 사용 형태: 터미널에서의 인터랙티브 Claude Code 세션.
- **알려진 버그**: 일부 사용자에게서 `ANTHROPIC_API_KEY`가 설정되지 않았는데도 `claude -p` 헤드리스 모드가 Max 구독 대신 API 요금으로 자동 전환되는 사례가 보고됨.
- **커뮤니티 보고 (단일 출처, 미확인)**: 커밋 메시지나 프로젝트 파일에 "Hermes" 같은 서드파티 하네스 시그니처 문자열이 남아 있으면 Anthropic 백엔드 분류기가 이를 감지해 API 과금으로 전환할 수 있다는 주장이 있고, 이로 인해 $200이 청구된 사례가 언급됨. 공식 확인 없음.
- 권장 점검 방법: `claude /status` 실행, `echo $ANTHROPIC_API_KEY` 확인, platform.claude.com에서 예상치 못한 API 사용량 확인, 프로젝트 파일에 하네스 이름 문자열이 남아있는지 스캔.
- 원 저자는 이후 인터랙티브 tmux 세션 기반으로 설정을 재작업 중이라고 언급(후속 게시물 예고, 이 클리핑에는 후속 내용 없음).

## Connections

- [[claude-code]]
- [[hermes-agent]]
- [[multi-agent-role-specialization]]

## Open Questions

- 오늘 날짜(2026-07-08) 기준으로 2026-06-15 정책이 실제 시행되었는지, 세부 크레딧 정책이 변경되지 않았는지 공식 소스로 재확인 필요 — needs-update.
- "Hermes" 문자열 감지로 인한 자동 API 과금 전환 주장은 단일 커뮤니티 댓글 근거이며 공식 문서로 뒷받침되지 않음.
- 이 노트는 시간 민감성이 높으므로 새로운 소스가 들어오면 우선 갱신 대상.
