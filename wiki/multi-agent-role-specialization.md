---
type: pattern
topics:
  - ai-agents
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
created: "2026-07-08"
updated: "2026-07-08"
---

# Multi-Agent Role Specialization

## Summary

하나의 모델/채팅창이 모든 것을 처리하도록 강요하지 않고, 역할별로 가장 적합한 도구를 배정하는 멀티 에이전트 구성 패턴. 예시 스택: 오케스트레이터([[hermes-agent|Hermes]]), 메인 에이전트 두뇌([[openai-codex|OpenAI Codex]]), 코딩 전문가([[claude-code|Claude Code]]), 인터페이스(Telegram), 실행 환경(로컬 머신).

## Details

- 워크플로우: 사용자가 오케스트레이터에게 요청 → 오케스트레이터가 직접 처리할지 코딩 전문가에게 위임할지 판단 → 코딩 전문가가 작업 수행 → 오케스트레이터가 결과 확인/테스트/보고.
- 이 패턴이 단일 모델(예: Claude만 메인 두뇌로 사용)보다 더 잘 작동했다고 보고됨(단일 사례, 주관적 평가 — needs-update).
- 성능이 낮은 하드웨어에서 로컬 LLM(Ollama 등)으로 모든 역할을 처리하려는 시도보다 안정적이었다고 언급됨.
- 커뮤니티 댓글에서 변형 사례: Opus 4.7이 다음 작업 청크를 정의하고 GPT-5.5가 구현, Sonnet 4.6이 빠른 리뷰, GPT-5.5가 마일스톤 단위 QA 검토하는 TDD + JIT + 수직 슬라이스 조합도 언급됨.
- 반복 사용 시 오케스트레이터의 스킬 시스템이 코딩 작업 라우팅 패턴을 학습해 실행 속도가 빨라진다는 커뮤니티 의견 있음(단일 댓글, 근거 약함 — needs-update).

## Connections

- [[hermes-agent]]
- [[claude-code]]
- [[openai-codex]]
- [[claude-code-programmatic-billing-split]]

## Open Questions

- "스킬 시스템이 패턴을 학습한다"는 주장은 구체적 메커니즘 설명이 없어 검증 필요.
- 정량적 성능 비교(응답 속도, 비용, 정확도) 데이터는 출처에 없음.
