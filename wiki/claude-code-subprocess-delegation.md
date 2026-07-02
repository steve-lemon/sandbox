---
type: pattern
topics:
  - ai-agents
status: draft
sources:
  - "raw/드디어 나에게 딱 맞았던 AI 에이전트 설정 Hermes + OpenAI Codex + Claude Code.md"
---

# Claude Code 서브프로세스 위임 패턴

제3자 오케스트레이터(예: [[hermes-agent|Hermes]])가 Anthropic API를 직접 호출하지 않고, `claude` CLI를 서브프로세스로 셸 아웃(shell out)하여 Claude Code의 코딩 능력을 구독(subscription) 기반으로 사용하는 기법이다.

## 동작 방식

```
claude -p "여기에 작업" --max-turns 10
```

- 래퍼(wrapper) 없이 단순 서브프로세스 호출만으로 동작
- Claude Code CLI가 자체적으로 사용자의 Claude Max/Pro 구독에 대해 OAuth 인증을 처리하므로, 오케스트레이터는 Anthropic API를 전혀 건드리지 않음
- Anthropic 입장에서는 사용자가 터미널에서 직접 `claude -p "task"`를 입력하는 것과 동일하게 보임
- 되지 않는 것: 오케스트레이터가 Claude 구독 자체를 API로 직접 호출하는 모델 제공자로 사용하는 것 — 이는 허용되지 않는다고 언급됨. 서브프로세스로 `claude` CLI를 실행하는 것은 별개.

## 설정 시 주의점

- Hermes가 관리하는 Node 런타임은 `~/.hermes/node/bin/claude`에 바이너리를 설치하지만 기본 PATH에 없음 → `bashrc`에 추가하거나 `~/.local/bin/`에 심볼릭 링크 필요
- 더 긴 세션에는 `tmux` 세션에 들어가 Claude Code를 인터랙티브하게 실행하고 오케스트레이터가 모니터링하는 방식을 사용

## 청구(billing) 관련 리스크

이 패턴은 [[anthropic-programmatic-usage-billing-split|Anthropic의 프로그래매틱 사용 청구 정책 변경]]과 직접적으로 관련되며, 아래와 같은 우회 경로(및 그로 인한 의도치 않은 API 과금 리스크)가 커뮤니티에서 보고되었다 (status: needs-update — 단일 커뮤니티 소스, 공식 확인 필요).

- **`-p` 헤드리스 모드 버그**: 일부 사용자에게서 `ANTHROPIC_API_KEY`가 설정되지 않았음에도 `claude -p` 헤드리스 모드가 Max 구독 대신 API 요금으로 자동 전환되는 버그가 보고됨
- **하네스 시그니처 감지**: 커밋 메시지 등 페이로드에 "Hermes"와 같은 제3자 하네스 문자열이 포함되면 Anthropic 백엔드가 이를 제3자 하네스 사용으로 플래그하여 API로 과금한 사례가 보고됨 (문서화된 사례로 API 요금 $200 청구 언급)

### 권장 점검 방법 (원문 인용)

- `claude /status` 실행
- `echo $ANTHROPIC_API_KEY` 확인 — 설정 여부와 무관하게 실제 활성화된 경로 확인
- platform.claude.com에서 예상치 못한 API 사용량 확인
- 프로젝트 파일에 "Hermes" 등 하네스 문자열이 있는지 스캔

## 관련 문서

- [[hermes-agent]]
- [[multi-agent-role-specialization]]
- [[anthropic-programmatic-usage-billing-split]]
