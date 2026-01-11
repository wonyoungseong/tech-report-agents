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

### Step 1: 저장소 클론

```bash
git clone https://github.com/wonyoungseong/tech-report-agents.git
cd tech-report-agents
```

### Step 2: 프로젝트에 적용

#### 방법 A: 특정 프로젝트에 설치 (권장)

```bash
# 타겟 프로젝트로 이동
cd /path/to/your-project

# .claude 폴더 복사 (에이전트 + 커맨드)
cp -r /path/to/tech-report-agents/.claude ./

# CLAUDE.md 복사 (프로젝트 컨텍스트)
cp /path/to/tech-report-agents/CLAUDE.md ./

# 아티팩트 폴더 생성
mkdir -p docs/report-artifacts
```

#### 방법 B: 전역 설치 (모든 프로젝트에서 사용)

```bash
# 사용자 전역 .claude 폴더에 복사
cp -r /path/to/tech-report-agents/.claude/agents ~/.claude/
cp -r /path/to/tech-report-agents/.claude/commands ~/.claude/
```

### Step 3: 설치 확인

Claude Code에서 다음 명령어로 확인:

```bash
# 에이전트 목록 확인
/agents

# 다음 에이전트들이 보여야 합니다:
# - report-master
# - audience-analyst
# - structure-architect
# - content-writer
# - quality-inspector
# - style-consultant
# - ideation-helper
```

---

## 🚀 사용법

### 커스텀 커맨드로 시작 (권장)

Claude Code 채팅창에서 슬래시 명령어 입력:

```bash
# 전체 보고서 작성 워크플로우
/report:master 분기 실적 보고서 작성해줘

# 개별 에이전트 호출
/report:analyst 이 보고서의 독자를 분석해줘
/report:architect 문서 구조를 설계해줘
/report:writer 본문을 작성해줘
/report:inspector 이 문서를 검토해줘
/report:style 비유를 제안해줘
/report:ideation 빠진 내용을 찾아줘
```

### Subagent 직접 호출

```bash
# 전체 워크플로우
report-master subagent를 사용해서 프로젝트 중간 보고서 작성해줘

# 개별 에이전트
quality-inspector subagent로 이 문서를 검토해줘
ideation-helper subagent로 빠진 내용 분석해줘
```

---

## 📂 설치 후 폴더 구조

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
│   │   └── ideation-helper.md     # 아이디어 발굴
│   └── commands/                  # ⭐ 커스텀 커맨드
│       ├── report:master.md       # /report:master
│       ├── report:analyst.md      # /report:analyst
│       ├── report:architect.md    # /report:architect
│       ├── report:writer.md       # /report:writer
│       ├── report:inspector.md    # /report:inspector
│       ├── report:style.md        # /report:style
│       └── report:ideation.md     # /report:ideation
├── docs/
│   └── report-artifacts/          # 워크플로우 산출물 (자동 생성)
└── CLAUDE.md                      # 프로젝트 컨텍스트
```

---

## 🎯 빠른 시작 예제

### 예제 1: 주간 보고서 작성

```bash
# Claude Code에서 입력
/report:master 이번 주 프로젝트 진행 상황 주간 보고서 작성해줘
```

**결과**: `docs/report-artifacts/YYYYMMDD_project-name/` 폴더에 아티팩트 생성

### 예제 2: 기존 문서 검토

```bash
# Claude Code에서 입력
/report:inspector docs/my-report.md 파일을 검토해줘
```

### 예제 3: 문서에 빠진 내용 찾기

```bash
# Claude Code에서 입력
/report:ideation 이 보고서에 빠진 내용이 뭔지 분석해줘
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

---

## 📋 커스텀 커맨드 목록

| 커맨드 | 에이전트 | 용도 |
|-------|---------|-----|
| `/report:master` | report-master | 전체 워크플로우 시작 |
| `/report:analyst` | audience-analyst | 독자 분석 |
| `/report:architect` | structure-architect | 구조 설계 |
| `/report:writer` | content-writer | 본문 작성 |
| `/report:inspector` | quality-inspector | 품질 검증 |
| `/report:style` | style-consultant | 스타일 조언 |
| `/report:ideation` | ideation-helper | 아이디어 발굴 |

---

## 🔀 스마트 라우팅

잘못된 에이전트를 호출하면 자동으로 적합한 에이전트를 추천합니다.

```bash
# 예: /report:style에게 "보고서 작성해줘" 요청 시

🔀 **에이전트 추천**

요청하신 "보고서 작성해줘"는 **report-master**가 더 적합합니다.

### 추천: report-master

**주요 기능**:
- 전체 워크플로우 조율
- 피드백 루프 관리

**진행 방법**:
> /report:master 보고서 작성해줘

해당 에이전트로 작업을 전달할까요? (네/아니요)
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

```markdown
# /command-name - 커맨드 설명

실행할 내용을 작성합니다.
```

---

## ❓ 문제 해결

### 에이전트가 보이지 않는 경우

1. `.claude/agents/` 폴더가 프로젝트 루트에 있는지 확인
2. Claude Code를 재시작
3. `/agents` 명령어로 확인

### 커맨드가 작동하지 않는 경우

1. `.claude/commands/` 폴더가 프로젝트 루트에 있는지 확인
2. 파일명이 `report:master.md` 형식인지 확인 (콜론 포함)
3. Claude Code를 재시작

### 아티팩트 폴더가 생성되지 않는 경우

```bash
mkdir -p docs/report-artifacts
```

---

## 📚 기반 지식

- **BMad Method v6**: 멀티 에이전트 아키텍처 철학
- **엔지니어를 위한 보고서 작성기술**: 한국어 기술 문서 작성 규칙

---

## 📄 라이선스

MIT License
