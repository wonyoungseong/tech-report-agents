# /report:architect - 문서 구조 설계

보고서의 계층 구조와 논리적 흐름을 설계합니다.

## 역할
structure-architect subagent를 호출하여 문서 구조를 설계합니다.

## 사용법
```
/report:architect [대상 문서 또는 주제]
```

## 예시
```
/report:architect docs/my-report.md의 구조를 분석해줘
/report:architect "분기 실적 보고서" 개요를 잡아줘
/report:architect 이 문서의 구조를 개선해줘
```

## 실행

structure-architect subagent를 사용하여 문서 구조를 설계하세요.

### 핵심 원칙
1. **General → Particular**: 결론/핵심 먼저, 상세는 나중
2. **한 문서 한 주제**: 복수 주제 금지
3. **계층 구조**: 문서 → 장 → 절 → 항 → 단락 → 문장

### 단락 배열 전략
| 유형 | 목적 | 패턴 |
|-----|-----|-----|
| 직렬형 | 설득 | A → B → C → D (원인 → 과정 → 결과) |
| 병렬형 | 정보 전달 | 논제문 후 a, b, c 독립 설명 |

### 템플릿 종류
- **일일 보고**: 5W1H
- **주간 보고**: 평가+계획
- **중간 보고**: 7란 구조
- **상세 보고서**: 11개 구성
- **문제 대책 시트**: 5항목

### 출력
- `02_structure-blueprint.md` (워크플로우 모드)
- `{주제}_structure.md` (단일 모드)
