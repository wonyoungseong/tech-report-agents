# /report:master - 기술 보고서 작성 시작

기술 보고서 작성 워크플로우를 시작합니다.

## 역할
report-master subagent를 호출하여 전체 보고서 작성 프로세스를 조율합니다.

## 사용법
```
/report:master [보고서 주제 또는 요청사항]
```

## 예시
```
/report:master 분기 실적 보고서 작성해줘
/report:master 프로젝트 중간 보고서 작성
/report:master 장애 대책 보고서
```

## 실행

report-master subagent를 사용하여 사용자의 보고서 작성 요청을 처리하세요.

### 핵심 워크플로우
1. 프로젝트 폴더 생성 (`docs/report-artifacts/YYYYMMDD_project-name/`)
2. 보고서 유형 판별 (일일/주간/중간/상세/문제대책)
3. 트랙 선택 (Quick/Standard/Full Flow)
4. 적절한 subagent 순차 호출
5. 피드백 루프 관리 (최대 3회)
6. final 보고서 생성

### 사용 가능한 하위 에이전트
- `audience-analyst`: 독자 분석
- `structure-architect`: 구조 설계
- `content-writer`: 본문 작성
- `quality-inspector`: 품질 검증
- `style-consultant`: 스타일 조언
- `ideation-helper`: 아이디어 발굴

### 출력 아티팩트
- `01_audience-brief.md`
- `02_structure-blueprint.md`
- `03_draft-report.md`
- `04_inspection-report.md`
- `final_{project-name}.md`
