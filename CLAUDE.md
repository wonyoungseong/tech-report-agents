# Technical Report Writing System

이 프로젝트는 기술 보고서 작성을 위한 멀티 에이전트 시스템입니다.

## 📁 프로젝트 구조

```
your-project/
├── .claude/
│   └── agents/           # Subagent 정의 파일
│       ├── report-master.md      # 오케스트레이터
│       ├── audience-analyst.md   # 독자 분석
│       ├── structure-architect.md # 구조 설계
│       ├── content-writer.md     # 본문 작성
│       ├── quality-inspector.md  # 품질 검증
│       └── style-consultant.md   # 스타일 조언
├── docs/
│   └── report-artifacts/  # 워크플로우 아티팩트
│       └── YYYYMMDD_project-name/  # 프로젝트별 폴더
│           ├── 01_audience-brief.md
│           ├── 02_structure-blueprint.md
│           ├── 03_draft-report.md
│           ├── 04_inspection-report.md
│           └── final_project-name.md
└── CLAUDE.md              # 이 파일
```

## 📂 폴더 관리 규칙

### 프로젝트 폴더 생성

보고서 작성 시작 시 프로젝트 폴더가 자동 생성됩니다:

```bash
# 폴더 생성 명령 (report-master가 실행)
mkdir -p docs/report-artifacts/YYYYMMDD_project-name
```

**폴더가 없는 경우**: 자동 생성
**폴더가 있는 경우**: 기존 폴더 사용 (덮어쓰기 전 확인)

### 네이밍 컨벤션

#### 폴더명
```
{YYYYMMDD}_{project-name}

규칙:
- YYYYMMDD: 프로젝트 시작일 (예: 20260111)
- project-name: 케밥케이스 (소문자, 하이픈)
- 공백 금지, 특수문자 금지

예시:
- 20260111_quarterly-sales-report
- 20260115_incident-analysis
- 20260120_project-status-update
```

#### 파일명
```
{순번}_{artifact-type}.md
{순번}_{artifact-type}_v{N}.md  # 수정 버전

순번:
- 01: audience-brief (분석)
- 02: structure-blueprint (설계)
- 03: draft-report (초안)
- 04: inspection-report (검토)
- 05: style-guide (스타일, 선택)
- final: 최종 산출물

예시:
- 01_audience-brief.md
- 03_draft-report.md
- 03_draft-report_v2.md    # 1차 수정
- 03_draft-report_v3.md    # 2차 수정
- final_quarterly-sales-report.md
```

### 버전 관리

수정이 발생할 때마다 버전 번호 증가:

```
수정 흐름:
03_draft-report.md     → (검토) → 04_inspection-report.md
       ↓ 수정 필요
03_draft-report_v2.md  → (재검토) → 04_inspection-report_v2.md
       ↓ 추가 수정
03_draft-report_v3.md  → (최종 승인) → final_xxx.md
```

## 🚀 시작하기

### 전체 워크플로우로 시작
보고서 작성을 처음부터 시작하려면:

```
report-master subagent를 사용해서 [보고서 종류] 작성해줘
```

예시:
```
report-master subagent를 사용해서 분기 실적 보고서 작성해줘
report-master subagent를 사용해서 프로젝트 중간 보고서 작성해줘
```

### 단일 에이전트로 시작 ⭐

이미 작성된 문서가 있거나, 특정 작업만 필요할 때:

```bash
# 기존 문서 검토
> quality-inspector subagent로 docs/my-report.md 검토해줘

# 기존 문서 구조 분석
> structure-architect subagent로 이 문서의 구조를 분석해줘

# 기존 문서 수정
> content-writer subagent로 이 문서를 작성 규칙에 맞게 수정해줘

# 독자 분석만
> audience-analyst subagent로 이 보고서의 독자층을 분석해줘

# 스타일 조언만
> style-consultant subagent로 이 기술 용어에 비유를 제안해줘
```

## 🤖 사용 가능한 Subagents

| Subagent | 역할 | 호출 키워드 |
|----------|-----|-----------|
| `report-master` | 전체 조율, 워크플로우 관리 | 보고서, 리포트, 문서 작성 |
| `audience-analyst` | 독자 분석, 요구사항 정의 | 독자 분석, 누구를 위한 |
| `structure-architect` | 문서 구조 설계 | 구조 설계, 개요, 목차 |
| `content-writer` | 본문 작성 | 본문 작성, 내용 작성 |
| `quality-inspector` | 품질 검증, 악문 진단, AI 파싱 친화도 | 검토, 리뷰, 피드백 |
| `style-consultant` | 문체 조언 | 스타일, 비유, 표현 개선 |
| `ideation-helper` | 아이디어 발굴, 누락 내용 제안 ⭐ | 빠진 내용, 아이디어, 보완 |

## 📊 워크플로우

### Quick Flow (일일/주간 보고)
```
report-master → audience-analyst → content-writer → quality-inspector
                                          ↑              ↓
                                          └── (피드백 루프) ──┘
                                                    ↓ approved
                                               [final 생성]
```

### Standard Flow (중간 보고)
```
report-master → audience-analyst → structure-architect → content-writer → quality-inspector
                                                               ↑              ↓
                                                               └── (피드백 루프) ──┘
                                                                         ↓ approved
                                                                    [final 생성]
```

### Full Flow (상세 보고서)
```
report-master → audience-analyst → structure-architect → content-writer → quality-inspector
                                                               ↑              ↓
                                           style-consultant ←──┤      (피드백 루프)
                                                  ↓            │              ↓
                                           content-writer ─────┘         [final 생성]
```

## 🔄 피드백 루프 시스템

### 동작 원리

report-master가 피드백 루프를 컨트롤합니다:

```
1. content-writer가 초안 작성 (03_draft-report.md)
2. quality-inspector가 검토 (04_inspection-report.md)
3. 판정 확인:
   - "approved" → final 생성
   - "revision_required" → 4단계로
4. content-writer가 피드백 반영 수정 (03_draft-report_v2.md)
5. quality-inspector가 재검토 (04_inspection-report_v2.md)
6. 2-5 반복 (최대 3회)
```

### 루프 제어

```yaml
max_iterations: 3        # 최대 반복 횟수
exit_conditions:
  - approved 판정
  - 3회 반복 도달 (사용자 판단 요청)
```

### 에이전트 간 참조 파일

| 에이전트 | 참조 파일 | 출력 파일 |
|---------|---------|----------|
| content-writer (초안) | 01_*.md, 02_*.md | 03_draft-report.md |
| content-writer (수정) | 03_*.md, 04_*.md | 03_draft-report_v{N}.md |
| quality-inspector | 03_*.md | 04_inspection-report.md |
| quality-inspector (재검토) | 03_*_v{N}.md | 04_inspection-report_v{N}.md |

## ✍️ 작성 규칙 (핵심)

### 4대 원칙
1. **한 문서 한 주제** - 복수 주제 금지
2. **한 단락 한 논제** - 옆길로 새지 않도록
3. **한 문장 한 논리** - 130자 이내
4. **한 단어 한 의미** - 애매함 금지

### 금지 표현
- `~등` → 구체적 나열
- `약간/조금/상당히` → 구체적 수치
- `빠른 시일 내에` → 구체적 일자
- `검토한다/노력한다` → 구체적 행동

### 들여쓰기 규칙
```markdown
# 일관된 단위 사용 (2칸 또는 4칸)
- 1단계
  - 2단계 (2칸)
    - 3단계 (4칸)
      - 4단계 (6칸) - 최대 권장

# 금지
❌ 불규칙 들여쓰기 (2칸/3칸/4칸 혼용)
❌ 5단계 이상 중첩
```

### Human + AI Readable 구조 ⭐

사람도 읽기 좋고 AI도 파싱하기 좋은 문서:

```markdown
# 핵심 원칙
1. 자기 완결적 섹션 - 각 섹션이 독립적으로 이해 가능
2. 명시적 관계 - "그것" → 구체적 명사
3. 구조화된 데이터 - 서술형보다 테이블/Key-Value
4. 요약 선행 - 각 섹션 시작에 요약

# 피해야 할 표현
❌ "위에서 언급한" → "2.1절에서 설명한"
❌ "그것을 적용" → "A/B 테스트 방법론을 적용"
❌ 긴 서술형 나열 → 테이블 또는 불릿 포인트
```

### 문서 흐름
- **General → Particular** (결론 먼저, 상세는 나중에)

## 📄 아티팩트

각 단계에서 `docs/report-artifacts/{프로젝트폴더}/` 에 아티팩트가 생성됩니다:

| 순번 | 파일명 | 생성 에이전트 | 내용 |
|-----|-------|-------------|-----|
| 01 | `01_audience-brief.md` | audience-analyst | 독자 정보, 보고서 사양 |
| 02 | `02_structure-blueprint.md` | structure-architect | 문서 구조, 섹션 구성 |
| 03 | `03_draft-report.md` | content-writer | 보고서 초안 |
| 04 | `04_inspection-report.md` | quality-inspector | 악문 진단, 수정 제안 |
| 05 | `05_style-guide.md` | style-consultant | 문체 조언 (선택) |
| - | `final_{name}.md` | report-master | 최종 보고서 |

### 아티팩트 경로 예시

```
docs/report-artifacts/20260111_quarterly-report/
├── 01_audience-brief.md
├── 02_structure-blueprint.md
├── 03_draft-report.md
├── 03_draft-report_v2.md        # 수정본
├── 04_inspection-report.md
├── 04_inspection-report_v2.md   # 재검토
└── final_quarterly-report.md    # 최종
```

## 🔧 커스터마이징

### 새 템플릿 추가
`structure-architect.md`에 새 템플릿 섹션 추가

### 금지 표현 추가
`content-writer.md`와 `quality-inspector.md`의 금지 표현 목록 수정

### 검토 기준 조정
`quality-inspector.md`의 체크리스트 수정
