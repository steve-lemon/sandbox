---
type: tool
topics:
  - ai-agents
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# Claude Code

## Summary

Claude Code는 집중된 코딩 작업자(coding specialist) 역할로 멀티 에이전트 구성에 투입할 수 있는 Anthropic의 CLI 코딩 에이전트다. 이미 Claude 구독(Pro/Max)이 있다면 Anthropic API 크레딧을 소모하지 않고 구독 인증만으로 사용할 수 있다. [[hermes-agent|Hermes]] 같은 오케스트레이터가 `claude` CLI를 서브프로세스로 호출해 제한된 코딩 작업을 위임하고, 결과를 검증한 뒤 보고하는 패턴이 소개됨.

## Use Cases

- 오케스트레이터([[hermes-agent|Hermes]])가 `claude -p "task" --max-turns 10` 형태로 서브프로세스 호출해 코딩 작업 위임
- 별도 래퍼 없이 셸 아웃만으로 연동 가능
- 긴 세션은 tmux 안에서 인터랙티브하게 실행하고 오케스트레이터가 모니터링

## Setup Notes

- Claude Code 자체가 사용자의 Claude 구독(Max 등)에 OAuth로 인증되므로, 오케스트레이터가 Anthropic API를 직접 호출하지 않는 한 API 요금이 발생하지 않는다고 언급됨.
- 금지되는 것은 Claude 구독을 오케스트레이터의 "모델 제공자"로 API 직접 호출에 사용하는 것이며, `claude` CLI를 서브프로세스로 실행하는 것은 허용된다는 것이 커뮤니티 의견으로 언급됨(공식 정책 재확인 필요 — needs-update).
- **과금 관련 중요 변경사항**: 자세한 내용은 [[claude-code-programmatic-billing-split]] 참고.

## Related Concepts

- [[hermes-agent]]
- [[claude-code-programmatic-billing-split]]
- [[multi-agent-role-specialization]]

## Open Questions

- "Hermes" 같은 서드파티 하네스 시그니처가 페이로드에 있으면 Anthropic 분류기가 이를 감지해 API 요금으로 전환한다는 커뮤니티 보고가 있음(단일 소스, $200 청구 사례 언급) — 공식 확인 필요, needs-update.
