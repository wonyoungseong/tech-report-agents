# 📝 Technical Report Writing Agents for Claude Code

기술 보고서 작성을 위한 Claude Code Subagent 시스템입니다.

## ✨ 주요 기능

- **7개 전문 에이전트**: 역할별 특화된 AI 에이전트
- **3가지 워크플로우**: Quick / Standard / Full Flow
- **피드백 루프**: 품질 검증 → 수정 → 재검증 자동화 (최대 3회)
- **스마트 라우팅**: 잘못된 에이전트 호출 시 적합한 에이전트 자동 추천
- **커스텀 커맨드**: `/report:master` 등 슬래시 명령어 지원

---

## 📦 설치 방법

### 방법 1: 폴더 전체 복사 (권장)

```bash
# 1. 이 폴더를 프로젝트 루트에 복사
cp -r tech-report-agents/.claude /your-project/
cp tech-report-agents/CLAUDE.md /your-project/

# 2. 아티팩트 폴더 생성
mkdir -p /your-project/docs/report-artifacts
```

### 방법 2: 개별 에이전트만 복사

```bash
# 사용자 전역 에이전트로 설치 (모든 프로젝트에서 사용)
mkdir -p ~/.claude/agents
cp tech-report-agents/.claude/agents/*.md ~/.claude/agents/
```

---

## 📂 폴더 구조

```
your-project/
├── .claude/
│   ├── agents/                    # ⭐ Subagent 정의
│   │   ├── report-master.md       # 오케스트레이터
│   │   ├── audience-analyst.md    # 독자 분석
│   │   ├── structure-architect.md # 구조 설계
│   │   ├── content-writer.md      # 본문 작성
│   │   ├── quality-inspector.md   # 품질 검증
│   │   ├── style-consultant.md    # 스타일 조언
│   │   └── ideation-helper.md     # 아이디어 발굴 ⭐ NEW
│   └── commands/                  # 커스텀 커맨드
│       └── report.md              # /report 명령어
├── docs/
│   └── report-artifacts/          # 워크플로우 산출물
│       └── YYYYMMDD_project-name/
│           ├── 01_audience-brief.md
│           ├── 02_structure-blueprint.md
│           ├── 03_draft-report.md
│           ├── 04_inspection-report.md
│           ├── 05_style-guide.md
│           └── final_project-name.md
└── CLAUDE.md                      # 프로젝트 컨텍스트
```

---

## 🚀 사용법

### 커스텀 커맨드 (권장)

```bash
# 전체 워크플로우 시작
/report:master 분기 실적 보고서 작성해줘

# 개별 에이전트 호출
/report:writer 본문 작성해줘
/report:inspector 이 문서 검토해줘
/report:style 비유 제안해줘
/report:ideation 빠진 내용 찾아줘
```

### Subagent 직접 호출

```bash
# 전체 워크플로우
> report-master subagent를 사용해서 프로젝트 중간 보고서 작성해줘

# 개별 에이전트
> audience-analyst subagent로 이 보고서의 독자를 분석해줘
> quality-inspector subagent로 이 문서를 검토해줘
> ideation-helper subagent로 빠진 내용 분석해줘
```

---

## 🔄 워크플로우 트랙

### Quick Flow (일일/주간 보고)
```
report-master → audience-analyst → content-writer → quality-inspector → final
```

### Standard Flow (중간 보고)
```
report-master → audience-analyst → structure-architect → content-writer
    → quality-inspector ⇄ (피드백 루프) → final
```

### Full Flow (상세 보고서)
```
report-master → audience-analyst → structure-architect → content-writer
    → quality-inspector ⇄ (피드백 루프) → style-consultant → final
```

### 피드백 루프

```
┌─────────────────────────────────────────────┐
│  content-writer → quality-inspector         │
│       ↑                    │                │
│       │              ┌─────┴─────┐          │
│       │              │ approved? │          │
│       │              └─────┬─────┘          │
│       │                Yes │ No             │
│       │                    │  │             │
│       └────────────────────┘  │             │
│                               ▼             │
│                         [최대 3회 반복]      │
│                               │             │
│                         3회 도달 시         │
│                         사용자 판단 요청     │
└─────────────────────────────────────────────┘
```

---

## 🔀 스마트 라우팅

잘못된 에이전트를 호출하면 자동으로 적합한 에이전트를 추천합니다.

```bash
# 예: style-consultant에게 "보고서 작성해줘" 요청 시

🔀 **에이전트 추천**

요청하신 "보고서 작성해줘"는 **report-master**가 더 적합합니다.

### 추천: report-master

**주요 기능**:
- 전체 워크플로우 조율
- 피드백 루프 관리

**진행 방법**:
> "report-master subagent로 보고서 작성해줘"

해당 에이전트로 작업을 전달할까요? (네/아니요)
```

---

## 📋 Subagent 상세

| Agent | Model | 역할 | 키워드 |
|-------|-------|-----|-------|
| report-master | sonnet | 전체 조율, 피드백 루프 | 보고서, 리포트, 문서 작성 |
| audience-analyst | sonnet | 독자 분석, 유형 결정 | 독자, 누구를 위한 |
| structure-architect | sonnet | 구조 설계 | 구조, 개요, 목차 |
| content-writer | sonnet | 본문 작성/수정 | 본문, 내용 작성 |
| quality-inspector | sonnet | 품질 검증, 악문 진단 | 검토, 리뷰, 피드백 |
| style-consultant | haiku | 문체, 비유 제안 | 스타일, 비유, 표현 |
| ideation-helper | sonnet | 누락 내용 발굴 | 빠진 내용, 아이디어, 보완 |

---

## 📁 폴더 관리 규칙

### 자동 폴더 생성

`report-master`가 프로젝트 시작 시 자동으로 폴더를 관리합니다:

```bash
# 폴더가 없으면 → 자동 생성
mkdir -p docs/report-artifacts/YYYYMMDD_project-name

# 폴더가 있으면 → 기존 폴더 사용
```

### 네이밍 컨벤션

#### 폴더명
```
{YYYYMMDD}_{project-name}

예시:
✅ 20260111_quarterly-sales-report
✅ 20260115_incident-analysis
❌ 20260111_Quarterly Sales Report (공백, 대문자)
```

#### 파일명 및 버전 관리
```
{순번}_{artifact-type}.md
{순번}_{artifact-type}_v{N}.md  # 수정 버전

예시:
- 03_draft-report.md      # 최초
- 03_draft-report_v2.md   # 1차 수정
- 03_draft-report_v3.md   # 2차 수정
```

---

## ✍️ 적용되는 작성 규칙

### 4대 원칙
| 원칙 | 설명 |
|-----|-----|
| 한 문서 한 주제 | 복수 주제 금지 |
| 한 단락 한 논제 | 논제문은 단락 첫머리 |
| 한 문장 한 논리 | **130자 이내** |
| 한 단어 한 의미 | 애매함 금지 |

### 금지 표현
| 금지 | 대체 |
|-----|-----|
| ~등 | 구체적 나열 |
| 약간/조금/상당히 | 구체적 수치 |
| 빠른 시일 내에 | 구체적 일자 (예: 1월 15일까지) |
| 검토한다/노력한다 | 구체적 행동 |

### AI 파싱 친화도 (9.5점 만점)
| 항목 | 배점 |
|-----|-----|
| 자기 완결적 섹션 | 2점 |
| 명시적 관계 ("그것" → 구체적 명사) | 2점 |
| 대명사 최소화 | 1점 |
| 구조화된 데이터 (테이블/Key-Value) | 2점 |
| 헤딩 계층 순서 | 1점 |
| 요약 선행 | 1점 |
| 메타데이터 | 0.5점 |

---

## 🔧 커스터마이징

### 에이전트 수정
`.claude/agents/` 폴더의 `.md` 파일을 직접 수정

### 새 에이전트 추가
`.claude/agents/new-agent.md` 파일 생성 (YAML frontmatter 포함)

```yaml
---
name: new-agent
description: 에이전트 설명
tools: Read, Write, Glob, Grep
model: sonnet
---

# Agent Name

## Identity
...
```

### 커스텀 커맨드 추가
`.claude/commands/` 폴더에 `.md` 파일 생성

---

## 📚 기반 지식

- **BMad Method v6**: 멀티 에이전트 아키텍처 철학
- **엔지니어를 위한 보고서 작성기술**: 한국어 기술 문서 작성 규칙

---

## 📄 라이선스

MIT License
