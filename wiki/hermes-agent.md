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

# Hermes Agent

## Summary

Hermes는 항상 켜져 있는(always-on) AI 오케스트레이터 에이전트다. 메모리, 도구, 예약 작업(cron), 메시징을 보유하고 실제로 컴퓨터에서 작업(이메일 전송, 스크립트 실행, 파일 확인, Telegram 소통 등)을 수행할 수 있다. 하나의 모델이 모든 역할을 다 하게 만드는 대신, Hermes는 조정자 역할만 맡고 코딩처럼 전문성이 필요한 작업은 [[claude-code|Claude Code]] 같은 전문 에이전트에 위임하는 구성에서 핵심 축이 된다.

## Use Cases

- 사용자 요청을 받아 직접 처리할지, 코딩 작업을 [[claude-code|Claude Code]]에 위임할지 판단
- cron, 이메일, 쉘, Home Assistant 등 로컬 도구 실행
- Telegram을 통한 원격 인터페이스 제공
- `claude` CLI를 서브프로세스로 호출해 코딩 작업을 위임하고 결과를 검증/보고 (`claude -p "task" --max-turns 10`)

## Setup Notes

- Hermes가 관리하는 Node 배포판은 `~/.hermes/node/bin/claude`에 `claude` 바이너리를 두는데 기본 PATH에 없다. `~/.local/bin/`에 심볼릭 링크하거나 PATH에 추가해야 Hermes가 찾을 수 있다.
- OpenAI 인증은 ChatGPT Pro OAuth 로그인을 사용하면 API 키 없이 정액제로 Hermes 메인 두뇌([[openai-codex|OpenAI Codex]])를 구동할 수 있다.
- Hermes가 Anthropic API와 직접 통신하지 않고 `claude` CLI를 서브프로세스로 실행하는 한, Anthropic 쪽에서는 사용자가 터미널에서 직접 명령을 입력하는 것과 동일하게 보인다고 언급됨. 단, [[claude-code-programmatic-billing-split|프로그래매틱 사용 과금 분리]] 이슈를 함께 확인할 것.

## Related Concepts

- [[multi-agent-role-specialization]]
- [[claude-code]]
- [[claude-code-programmatic-billing-split]]

## Open Questions

- Hermes 자체의 메인 오케스트레이션 모델 선택 기준(커뮤니티 댓글에서 여러 사용자가 사용 모델을 질문함)은 출처에 명확히 나오지 않음 — needs-update.
