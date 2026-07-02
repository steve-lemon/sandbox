---
type: tool
topics:
  - ai-agents
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
---

# Hermes Agent

Hermes는 상시 실행되는(always-on) 개인용 AI 오케스트레이터 에이전트다. 단일 모델이 모든 작업을 처리하게 하는 대신, 메모리·도구·예약 작업(cron)·메시징을 보유하고 실제로 로컬 컴퓨터에서 작업을 수행하는 "관리자" 역할을 맡는다.

## 핵심 역할

- 이메일 전송, 스크립트 실행, 파일 확인, 쉘 명령, Home Assistant 연동 등 로컬 도구 사용
- [[claude-code-subprocess-delegation|Claude Code로의 코딩 작업 위임]] — 서브프로세스 호출을 통해 제한된 코딩 작업을 넘기고 결과를 검토
- 스케줄링(cron) 및 [[telegram-agent-interface|Telegram]]을 통한 원격 제어 인터페이스 제공
- 메인 두뇌 모델(예: OpenAI Codex)과 결합되어 일반적인 조정·도구 사용을 처리

Hermes 자체는 오케스트레이션 계층이며, 실제 추론을 담당하는 "메인 두뇌" 모델(OpenAI Codex 등)과 결합해 사용하는 것이 권장된다. 이 조합은 [[multi-agent-role-specialization|역할 분담형 멀티 에이전트 패턴]]의 대표 사례로 언급된다.

## 알려진 이슈

- Hermes가 관리하는 Node 런타임이 `~/.hermes/node/bin/claude`에 `claude` 바이너리를 설치하는데, 기본 PATH에 포함되지 않아 별도로 심볼릭 링크하거나 `PATH`에 추가해야 한다.
- Telegram 연동 시 `openai-codex` 제공자 인증 후 추론 단계에서 멈추거나 `'NoneType' object is not iterable` 오류가 발생하는 사례가 보고됨 — Hermes 자체 버그로, 최신 버전 업데이트로 해결된 사례가 있다 (status: needs-update, 원문 시점 이후 재현 여부 미확인).

## Status

- 원문 게시 시점(2026-05) 기준 정보이며 커뮤니티(Reddit) 공유 사례에 기반함 — 공식 문서 기반 검증 필요.
