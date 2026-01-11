# /report:inspector - 품질 검증

기술 보고서의 품질을 검증하고 악문을 진단합니다.

## 역할
quality-inspector subagent를 호출하여 품질 검증 및 개선점 제안을 수행합니다.

## 사용법
```
/report:inspector [검토할 문서]
```

## 예시
```
/report:inspector docs/my-report.md 검토해줘
/report:inspector 이 파일 검토해줘
/report:inspector 악문 진단해줘
```

## 실행

quality-inspector subagent를 사용하여 품질 검증을 수행하세요.

### 검증 레벨
1. **문서 레벨**: 한 문서 한 주제, 제목 예고 효과
2. **단락 레벨**: 한 단락 한 논제, 논제문 위치
3. **문장 레벨**: 130자 이내, 한 문장 한 논리
4. **단어 레벨**: 금지 표현 탐지, 용어 일관성
5. **형식 레벨**: 들여쓰기 일관성, 헤딩 계층
6. **AI 파싱 친화도**: 자기 완결적 섹션, 명시적 관계

### 악문 유형
| 유형 | 심각도 | 증상 |
|-----|-------|-----|
| 장문병 | 🔴 Critical | 130자 초과, 복수 논리 |
| 예고 없는 병 | 🟡 Major | 논제문 부재 |
| 결론 지연병 | 🟡 Major | 핵심이 끝에만 |
| 뒤틀린 문장 | 🔴 Critical | 주어-서술어 불일치 |
| 들여쓰기 혼란 | 🟡 Major | 단위 불일치 |

### 판정 기준
- **approved**: Critical 0건, 품질 8점 이상
- **revision_required**: Critical 1건 이상 또는 품질 8점 미만

### 출력
- `04_inspection-report.md` (검토 결과)
- `{원본파일명}_inspection.md` (단일 모드)
