---
type: tool
topics:
  - ai-agents
status: stub
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# OpenAI Codex

## Summary

이 소스에서 OpenAI Codex는 [[hermes-agent|Hermes]]의 메인 두뇌(main agent model) 역할로 사용된다. 일반적인 앞뒤 조정과 도구 사용을 담당하며, ChatGPT Pro 구독의 OAuth 로그인으로 실행해 API 토큰 과금 없이 사용할 수 있다고 언급됨.

## Use Cases

- Hermes의 메인 에이전트 두뇌로서 도구 사용 및 대화 조정 처리

## Setup Notes

- ChatGPT Pro 구독 OAuth 로그인으로 실행하면 API 키 없이 정액제로 사용 가능. 사용량이 많으면 속도 제한이 걸릴 수 있음.

## Related Concepts

- [[hermes-agent]]
- [[multi-agent-role-specialization]]

## Open Questions

- Codex 자체의 세부 설정/모델 버전은 출처에 명시되지 않음 — needs-update.
