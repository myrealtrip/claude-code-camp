# Claude Code Camp

Claude Code를 활용한 인터랙티브 강의 시리즈입니다.

An interactive lecture series for learning Claude Code.

## 강의 목록 / Lectures

| Day | 주제 / Topic | 소요 시간 / Duration |
|-----|-------------|---------------------|
| Day 1 | CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기 | 60분 |
| Day 2 | 나만의 Skill 만들기 — 도구에서 워크플로우까지 | 60분 |

## 설치 / Installation

### 사전 요구사항 / Prerequisites

- [Git](https://git-scm.com/)
- [GitHub CLI](https://cli.github.com/)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)

### 레포지토리 클론 / Clone the repository

```bash
git clone https://github.com/myrealtrip/claude-code-camp.git
cd claude-code-camp
```

## 실행 방법 / How to Run

### 강의 시작하기 / Start a lecture

Claude Code를 실행하고 슬래시 커맨드로 강의를 시작합니다.

Launch Claude Code and start a lecture with the slash command.

```bash
claude
```

Claude Code 안에서 아래 커맨드를 입력합니다:

Inside Claude Code, type the following command:

```
/camp-day1
/camp-day2
```

강의가 인터랙티브하게 진행됩니다. 각 섹션에서 설명, 실습, 퀴즈가 차례로 진행되며, 사용자의 페이스에 맞춰 진행됩니다.

The lecture runs interactively. Each section includes explanations, hands-on exercises, and quizzes, proceeding at your pace.

## 프로젝트 구조 / Project Structure

```
claude-code-camp/
├── README.md
├── day1/
│   ├── day1-lecture.md              # Day 1 강의 교안 / Lecture material
│   └── exercises/                   # 실습 템플릿 / Exercise templates
├── day2/
│   └── day2-lecture.md              # Day 2 강의 교안 / Lecture material
├── .claude/
│   ├── commands/
│   │   └── camp-skill-generator.md  # 교안 → 스킬 변환기 / Lecture-to-skill converter
│   └── skills/
│       ├── camp-day1/
│       │   └── SKILL.md             # Day 1 인터랙티브 스킬 / Day 1 interactive skill
│       └── camp-day2/
│           ├── SKILL.md             # Day 2 인터랙티브 스킬 / Day 2 interactive skill
│           ├── journey-diagram.html # 여정 다이어그램 / Journey diagram
│           ├── orchestration-diagram.html # 오케스트레이션 도식 / Orchestration diagram
│           └── .claude/skills/      # 공통 스킬 / Shared skills
│               ├── camp-os-detect/      # OS 자동 감지
│               ├── camp-lecture-start/  # 강의 시작 루틴
│               ├── camp-quiz/           # 퀴즈 출제/채점
│               └── camp-section-transition/ # 섹션 전환
└── .gitignore
```

## 기여 / Contributing

새로운 Day 강의를 추가하려면:

To add a new Day lecture:

1. `dayN/dayN-lecture.md` 형식으로 교안을 작성합니다 / Create a lecture file
2. `/camp-skill-generator` 커맨드로 스킬을 자동 생성합니다 / Auto-generate a skill with the command
