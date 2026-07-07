# projects/

프로젝트별 실행 문서·산출물 모음. 개념은 `wiki/`에, 실행 맥락만 여기에.

> **규칙:** 프로젝트를 추가·변경·완료할 때마다 아래 인덱스를 반드시 업데이트한다.

## 인덱스

| 프로젝트 | 상태 | 목적 |
|---------|------|------|
| [second-brain](second-brain/) | active | 이 vault의 구조·워크플로우 지속 개선 (3-루프 가동) |

> 상태·마감·다음 행동의 진실원은 각 프로젝트 README의 frontmatter다. 루트 `projects.base` 대시보드에서 집계된다. 완료(`done`) 프로젝트는 `archive/projects/`로 이동한다.

## 프로젝트 추가 방법

```
projects/<name>/
  README.md     ← 목적, 상태, 관련 wiki 링크
  outputs/      ← 이 프로젝트 전용 산출물
```

1. `projects/<name>/README.md` 작성 (목적·상태·관련 wiki 링크 포함)
2. `projects/<name>/outputs/` 생성
3. **이 파일(projects/README.md) 인덱스 테이블 업데이트**
