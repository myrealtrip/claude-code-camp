---
description: "Day 1: CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기 - 인터랙티브 강의 진행"
---

# Day 1: CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기

이 스킬은 "CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기" 강의를 인터랙티브하게 진행합니다.

## 대상

- 개발자, 비개발 직군(마케터, 세일즈, 전략, 재무, 운영 등) 모두 포함
- Claude Code를 처음 쓰거나 단편적으로만 사용해본 분

## 진행 방식

강의를 섹션별로 진행합니다. 각 섹션에서:

1. 핵심 개념을 설명합니다
2. 실습이 있으면 사용자와 함께 직접 실행합니다
3. 퀴즈가 있으면 사용자에게 문제를 내고 답을 확인합니다
4. 사용자가 준비되면 다음 섹션으로 넘어갑니다

총 소요 시간: 약 60분

---

## 섹션별 진행 가이드

### 섹션 0: Why Claude Code? (5분)

**목표**: AI 도구들 사이에서 Claude Code를 선택하는 이유를 이해시킨다.

**진행 순서**:

1. "AI 도구들이 비슷해지고 있다"는 점을 설명한다
   - Claude Code, OpenAI Codex, Gemini CLI 모두 로컬 파일 접근, 터미널 실행, 자연어 대화를 지원
   - 모델 성능은 태스크와 시기마다 엎치락뒤치락
2. Claude Code의 차별점은 **확장성**임을 강조한다
   - MCP (Model Context Protocol): Slack, Jira, Notion, Gmail 등 외부 도구 연결
   - Skills: 반복 워크플로우를 명령어로 저장 (예: `/briefing`)
   - Subagents: 여러 AI에게 작업을 분업 (병렬 실행)
3. 핵심 메시지 전달: **"모델은 교체할 수 있지만, 워크플로우는 자산으로 남는다."**
4. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 1: Setup - 환경 준비 (15분)

**목표**: Git, GitHub CLI, Claude Code가 모두 설치되고 동작하는 상태를 만든다.

**진행 순서**:

1. **GitHub 계정 확인** — 이미 있는지 물어보고, 없으면 https://github.com 가입을 안내한다
2. **Git 설치 확인** — 사용자에게 `git --version` 실행을 요청한다
   - 설치되어 있으면 건너뛴다
   - 없으면 OS에 맞는 설치 가이드를 안내한다:
     - macOS: `brew install git`
     - Windows: https://git-scm.com/download/win
   - 설치 후 기본 설정을 안내한다:
     ```
     git config --global user.name "이름"
     git config --global user.email "이메일"
     ```
3. **GitHub CLI 설치 확인** — `gh --version` 실행을 요청한다
   - 없으면 설치 안내: macOS `brew install gh`, Windows https://cli.github.com
   - `gh auth login`으로 로그인을 안내한다 (HTTPS, Web browser 선택)
4. **Claude Code 설치 확인** — `claude --version` 실행을 요청한다
   - 없으면 `npm install -g @anthropic-ai/claude-code` 안내
5. **최종 체크리스트** — 세 명령어를 한 번에 확인하도록 안내한다:
   ```
   git --version && gh --version && claude --version
   ```
6. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 2: CLAUDE.md - 나만의 매뉴얼 (25분)

**목표**: CLAUDE.md의 개념, Scope, 팀/개인 구분을 이해하고 직접 작성한다.

**진행 순서**:

#### 개념 설명 (5분)

1. CLAUDE.md란 무엇인지 설명한다
   - 새 팀원에게 주는 업무 가이드에 비유
   - Claude Code가 매 대화 시작 시 읽고 따르는 지시사항 파일
2. **Scope** 개념을 설명한다 — 위치에 따라 적용 범위가 달라짐
   - 글로벌 (`~/.claude/CLAUDE.md`): 모든 프로젝트에 적용 → 회사 전체 규칙
   - 프로젝트 (`./CLAUDE.md`): 이 프로젝트에서만 적용 → 부서 규칙
   - 하위폴더 (`./하위폴더/CLAUDE.md`): 그 폴더 안에서만 적용 → 특정 업무 규칙
3. **팀 공유 vs 개인용** 구분을 설명한다
   - `./CLAUDE.md`: 팀 공유 (git에 올라감)
   - `./CLAUDE.local.md`: 개인용 (git에 안 올라감)
   - `~/.claude/CLAUDE.md`: 글로벌 개인용
4. 유용한 명령어를 소개한다: `/init`, `#` + 지시사항, `/memory`

#### 퀴즈 (3분)

AskUserQuestion으로 아래 퀴즈를 **하나씩** 출제한다:

**Q1. 모든 프로젝트에서 Claude가 한국어로 답하게 하려면 CLAUDE.md를 어디에 둘까요?**
- A) 프로젝트 폴더 안
- B) `~/.claude/CLAUDE.md`
- C) 아무 데나
- 정답: **B** — 글로벌 CLAUDE.md는 모든 프로젝트에 적용된다.

**Q2. 팀원은 모르게 내 개인 설정만 적용하고 싶으면?**
- A) `./CLAUDE.md`
- B) `./CLAUDE.local.md`
- C) 하위폴더에 CLAUDE.md 생성
- 정답: **B** — `CLAUDE.local.md`는 git에 올라가지 않아 개인 전용이다.

#### 실습 (17분)

사용자에게 단계별로 직접 실행하도록 안내한다. 막히면 도와준다.

**Step 1: 프로젝트 CLAUDE.md 만들기**
- 실습용 폴더 생성: `mkdir my-first-project && cd my-first-project && git init`
- CLAUDE.md 작성 (반말 규칙 포함)
- Claude Code 실행 후 "이 프로젝트에 대해 설명해줘" 질문
- 반말로 답하는지 확인

**Step 2: 글로벌 CLAUDE.md 추가하기**
- `~/.claude/CLAUDE.md` 생성 (존댓말 규칙 포함)
- Claude Code 재실행 후 `/memory`로 로딩된 파일 확인
- 글로벌(존댓말) vs 프로젝트(반말) 충돌 시 프로젝트가 우선하는 것을 확인

**Step 3: 하위폴더 CLAUDE.md로 Scope 체험**
- `reports/CLAUDE.md` 생성 (보고서 모드 규칙)
- `/memory`로 3개 CLAUDE.md 로딩 확인
- "reports 폴더에 월간 보고서를 작성해줘" 테스트

**Step 4: git으로 기록**
- `git add CLAUDE.md reports/CLAUDE.md && git commit -m "프로젝트 CLAUDE.md 추가"`

각 Step이 끝날 때마다 AskUserQuestion으로 진행 상황을 확인한다.

---

### 섹션 3: Memory - 기억하는 Claude (15분)

**목표**: Memory의 개념과 CLAUDE.md와의 차이를 이해하고, 직접 Memory를 사용해본다.

**진행 순서**:

#### 개념 설명 (5분)

1. Memory란 무엇인지 설명한다
   - CLAUDE.md가 "내가 쓰는 매뉴얼"이라면, Memory는 "Claude가 스스로 쓰는 업무 일지"
   - 대화 중 Claude가 유용하다고 판단한 정보를 자동 저장
2. **CLAUDE.md vs Memory 비교표**를 제시한다
   - 작성 주체: 내가 직접 vs Claude가 자동
   - 성격: 지시/규칙 vs 학습/메모
   - Scope: 3단계 vs 프로젝트 단위
   - 공유: 팀 공유 가능 vs 항상 개인
3. Memory 저장 위치를 설명한다: `~/.claude/projects/<프로젝트>/memory/`
4. Memory 관련 명령어를 소개한다: "기억해줘", "잊어줘", `/memory`

#### 퀴즈 (3분)

AskUserQuestion으로 출제한다:

**Q. 다음 중 CLAUDE.md에 넣을 것(📋)과 Memory에 맡길 것(🧠)을 구분하세요:**
1. "항상 존댓말 사용"
2. "이 프로젝트의 빌드 명령어는 npm run build"
3. "보고서는 A4 형식으로 작성"
4. "지난번 회의에서 디자인 변경이 결정됨"

- 4개 항목 중 선택할 수 있도록 multiSelect AskUserQuestion을 사용한다
- 정답: 1번 📋, 2번 🧠, 3번 📋, 4번 🧠
- 기준: 내가 정한 규칙이면 CLAUDE.md, Claude가 알아두면 좋은 사실/맥락이면 Memory

#### 실습 (7분)

**Step 1: Claude에게 기억 요청하기**
- Claude Code에서 "내 이름은 OOO이야. 기억해줘." 입력을 안내
- "Writing memory" 표시 확인

**Step 2: 새 대화에서 확인하기**
- Claude Code 종료 후 재시작
- "내 이름이 뭐였지?" 질문으로 기억 확인

**Step 3: Memory 파일 직접 확인**
- `ls ~/.claude/projects/`로 Memory 파일 위치 확인
- `/memory`로 Memory 파일 목록 확인
- Memory 파일은 일반 마크다운이라 직접 편집/삭제 가능함을 안내

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 4: Wrap-up - 오늘 배운 것 (5분)

**목표**: 학습 내용을 정리하고 종합 퀴즈로 확인한다.

**진행 순서**:

1. 오늘 배운 2가지를 정리한다:
   - **CLAUDE.md** = 업무 매뉴얼. 내가 쓰는 규칙. 위치(scope)에 따라 적용 범위가 달라짐
   - **Memory** = 업무 일지. Claude가 자동으로 쌓는 학습. 항상 개인용

2. AskUserQuestion으로 종합 퀴즈를 **하나씩** 출제한다:

**Q1. Claude가 매번 영어로 답하는데, 어떻게 고칠까요?**
- 정답: `~/.claude/CLAUDE.md`에 "한국어로 답변해주세요"를 추가한다. 글로벌 CLAUDE.md는 모든 프로젝트에 적용된다.

**Q2. 팀원과 프로젝트 규칙을 공유하고 싶으면?**
- 정답: 프로젝트 루트의 `./CLAUDE.md`에 작성하고 `git commit`한다. git을 통해 팀원과 공유된다.

**Q3. Claude가 전에 알려준 정보를 다음에도 기억하게 하려면?**
- 정답: "기억해줘"라고 말하면 된다. Claude가 Memory에 자동 저장한다.

3. 다음 단계를 안내한다:
   - 나의 글로벌 CLAUDE.md를 업무 스타일에 맞게 다듬기
   - 실제 프로젝트에 CLAUDE.md 추가해보기
   - Claude와 대화하면서 Memory가 쌓이는 것 관찰하기

---

## 진행 규칙

- AskUserQuestion 도구를 사용하여 퀴즈를 출제하세요. 선택지를 옵션으로 제공하세요.
- 퀴즈 정답은 사용자가 답한 후에 공개하세요. 맞으면 칭찬하고, 틀리면 왜 그 답이 정답인지 설명하세요.
- 실습 단계에서는 사용자가 직접 명령어를 실행하도록 안내하되, 막히면 도와주세요.
- 각 섹션이 끝나면 AskUserQuestion으로 "다음 섹션으로 넘어갈까요?"를 확인하세요.
- 사용자가 질문하면 교안 내용을 기반으로 답변하세요.
- 교안에 없는 내용이면 솔직히 말하고, 관련 레퍼런스를 안내하세요.
- 사용자의 OS(macOS/Windows)를 먼저 파악하고, 해당 OS에 맞는 안내만 하세요.
- 설명할 때는 비유를 적극 활용하세요. 교안에 있는 비유를 우선 사용하세요.
- 강의 시작 시 간단한 인사와 함께 강의 개요를 소개하고, 준비물이 갖춰져 있는지 확인하세요.
