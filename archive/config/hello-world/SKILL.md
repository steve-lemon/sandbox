---
name: hello-world
description: >
  이 vault의 Clippings 처리 파이프라인을 실행한다. "클리핑 처리", "새 소스 추가", "Clippings 파일 wiki로", "hello world 실행", "ingest", "소스 넣어줘", "다시 실행", "재실행", "업데이트", "결과 보완", "이전 결과 개선" 등 vault 콘텐츠 추가·수정 요청이 오면 반드시 이 스킬을 사용할 것. 단순 wiki 검색이나 쿼리는 해당하지 않는다.
---

# Hello World — Clippings 처리 오케스트레이터

Clippings/ → raw/ 이동 → 개념 추출 → wiki 아티클 작성의 전체 파이프라인을 순차 서브 에이전트로 실행한다.

**실행 모드:** 서브 에이전트 (Reader → Writer 순차 파이프라인)

> 파이프라인 패턴은 순차 의존이 강해 팀 통신 이점이 제한적이다. Reader 완료 후 Writer가 파일을 Read하는 구조이므로 서브 에이전트 모드가 적합.

## 에이전트 구성

| 에이전트 | subagent_type | 역할 | 출력 |
|---------|--------------|------|------|
| reader | 커스텀 | Clippings 파일 분석 및 개념 추출 | `_workspace/01_reader_analysis.md` |
| writer | 커스텀 | wiki 아티클 작성 + INDEX/topics/VAULT_MEMORY 업데이트 | `_workspace/02_writer_summary.md` |

## 데이터 흐름

```
[오케스트레이터]
    │
    ├── Agent(reader) → _workspace/01_reader_analysis.md
    │                              │
    └── Agent(writer) ←────────── Read
                   │
                   └── wiki/*.md
                       wiki/topics/*.md
                       wiki/INDEX.md
                       wiki/VAULT_MEMORY.md
                       _workspace/02_writer_summary.md
```

## 워크플로우

### Phase 0: 컨텍스트 확인

1. `_workspace/` 디렉토리 존재 여부 확인:
   - **미존재** → 초기 실행. Phase 1로 진행
   - **존재 + 부분 수정 요청** → 부분 재실행: 해당 에이전트만 재호출, 기존 산출물 중 수정 대상만 덮어씀
   - **존재 + 새 Clippings 파일** → 새 실행: 기존 `_workspace/`를 `_workspace_prev/`로 이동 후 Phase 1 진행
2. `Clippings/` 폴더에 미처리 파일이 있는지 확인한다. 없으면 사용자에게 알리고 종료.

### Phase 1: 준비

1. `_workspace/` 디렉토리 생성 (초기 실행 또는 이전 디렉토리 이동 직후)

### Phase 2: Reader 실행

```
Agent(
  subagent_type: "reader",
  model: "opus",
  prompt: "reader 에이전트 역할을 수행하라. Clippings/ 파일을 raw/ 로 이동하고 분석하여 _workspace/01_reader_analysis.md 를 생성하라. vault 경로: /Users/dujung/knowledge"
)
```

- Reader 반환 후 `_workspace/01_reader_analysis.md` 생성 여부 확인
- 파일이 없거나 "처리할 파일 없음" 내용이면 Writer 호출 없이 종료

### Phase 3: Writer 실행

```
Agent(
  subagent_type: "writer",
  model: "opus",
  prompt: "writer 에이전트 역할을 수행하라. _workspace/01_reader_analysis.md 를 읽고 wiki 아티클을 작성하라. INDEX.md, topics/, VAULT_MEMORY.md 를 모두 업데이트하라. vault 경로: /Users/dujung/knowledge"
)
```

### Phase 4: 정리 및 보고

1. `_workspace/` 보존 (중간 산출물 삭제하지 않음 — 사후 검증·감사 추적용)
2. `_workspace/02_writer_summary.md` 를 읽고 사용자에게 요약 보고:
   - 생성/업데이트된 아티클 목록
   - 처리된 소스 파일 목록
   - stub 상태 아티클 (확장 필요)
   - 토픽 분할 후보 (해당 시)

## 에러 핸들링

| 상황 | 전략 |
|------|------|
| Reader 실패 | 1회 재시도. 재실패 시 실패 내용 보고, Writer 건너뜀 |
| Writer 실패 | Reader 산출물 `_workspace/`에 보존, 실패 내용 보고 |
| `_workspace/01_reader_analysis.md` 없음 | Writer 미호출, 사용자에게 Reader 실패 알림 |

## 테스트 시나리오

### 정상 흐름
1. `Clippings/` 에 파일 1개 추가 → "클리핑 처리해줘" 입력
2. Phase 0: `_workspace/` 없음 → 초기 실행 분기
3. Phase 2: Reader가 파일을 `raw/`로 이동하고 개념 추출 → `_workspace/01_reader_analysis.md` 생성
4. Phase 3: Writer가 wiki 아티클 생성, INDEX.md / VAULT_MEMORY.md 업데이트
5. Phase 4: 생성 결과 요약 보고

### 에러 흐름
1. `Clippings/` 가 비어있는 상태에서 "클리핑 처리해줘" 입력
2. Phase 0에서 감지 → "처리할 파일이 없습니다" 메시지 출력 후 종료 (Writer 미호출)
