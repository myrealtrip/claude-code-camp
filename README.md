# Claude Code Camp

Claude Code 스킬 기반의 인터랙티브 교육 프로그램입니다.

Claude Code의 슬래시 커맨드(`/camp-day1` 등)를 통해 강의를 진행하며, 실습과 퀴즈를 포함한 핸즈온 방식으로 학습합니다.

## 강의 목록

| Day | 주제 | 소요 시간 | 난이도 |
|-----|------|-----------|--------|
| 1 | [CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기](day1/day1-lecture.md) | 약 60분 | 입문 |

## 사용법

### 1. 사전 준비

- Git, GitHub CLI(`gh`), Claude Code 설치

### 2. 강의 시작

Claude Code에서 슬래시 커맨드로 강의를 실행합니다:

```
/camp-day1
```

스킬이 강의를 섹션별로 진행하며, 실습 안내와 퀴즈 출제를 자동으로 수행합니다.

### 3. 새 강의 스킬 생성

강의 교안 마크다운을 작성한 뒤, 스킬 생성기로 인터랙티브 강의 스킬을 만들 수 있습니다:

```
/camp-skill-generator
```

## 프로젝트 구조

```
claude-code-camp/
├── day1/
│   ├── day1-lecture.md            # Day 1 강의 교안
│   └── exercises/                 # 실습 과제
├── .claude/
│   ├── commands/                  # 슬래시 커맨드 (스킬 생성기 등)
│   └── skills/                    # 강의 진행 스킬
│       └── camp-day1/
│           ├── SKILL.md           # Day 1 인터랙티브 강의 스킬
│           └── references/        # 강의 참고 자료
└── README.md
```

## 강의 추가 방법

1. `dayN/dayN-lecture.md` 형식으로 강의 교안을 작성합니다
2. `/camp-skill-generator` 명령으로 인터랙티브 스킬을 자동 생성합니다
3. `/camp-dayN` 으로 강의를 실행합니다
