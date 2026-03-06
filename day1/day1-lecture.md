# Day 1: CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기

> Claude Code를 나에게 맞게 설정하는 법을 배웁니다.
> 소요 시간: 약 60분 | 난이도: 입문

### 대상

- 개발자, 비개발 직군(마케터, 세일즈, 전략, 재무, 운영 등) 모두 포함
- Claude Code를 처음 쓰거나 단편적으로만 사용해본 분

### 오늘 다루는 내용

| 순서 | 주제 | 한 줄 요약 |
|------|------|-----------|
| 0 | Why Claude Code? | 왜 다른 AI 도구가 아닌 Claude Code인가 |
| 1 | Setup | Git, GitHub CLI, Claude Code 설치 및 설정 |
| 2 | CLAUDE.md | 내가 쓰는 규칙 — Claude에게 주는 업무 매뉴얼 |
| 3 | Memory | Claude가 쌓는 학습 — 기억하는 AI 비서 |
| 4 | Wrap-up | 정리 및 퀴즈 |

---

## 목차

- [0. Why Claude Code?](#0-why-claude-code)
- [1. Setup: 환경 준비](#1-setup-환경-준비)
- [2. CLAUDE.md: 나만의 매뉴얼](#2-claudemd-나만의-매뉴얼)
- [3. Memory: 기억하는 Claude](#3-memory-기억하는-claude)
- [4. Wrap-up: 오늘 배운 것](#4-wrap-up-오늘-배운-것)

---

## 준비물: 시작하기 전에

> 아래 내용을 강의 전에 미리 읽어두세요.

### 터미널 설치 (권장)

Claude Code는 터미널에서 실행합니다. 터미널은 텍스트로 컴퓨터에 명령을 내리는 창입니다.

기본 터미널도 동작하지만, 아래 터미널을 설치하면 탭 관리, 단축키, 가독성 등이 훨씬 좋습니다.

| OS | 권장 터미널 | 설치 / 실행 방법 |
|------|-----------|----------|
| macOS | [iTerm2](https://iterm2.com/) | 홈페이지에서 다운로드하거나 `brew install --cask iterm2` |
| Windows | PowerShell | 시작 메뉴 → "PowerShell" 검색 → 실행 (기본 설치되어 있음) |

### 기본 터미널 명령어

실습에서 자주 쓰는 명령어입니다. 외울 필요는 없고, 이런 게 있다는 것만 알아두세요.

| 명령어 | 하는 일 | 비유 | 예시 |
|--------|--------|------|------|
| `pwd` | 현재 위치 확인 | "나 지금 어디야?" | `pwd` → `/Users/나/문서` |
| `ls` | 현재 폴더의 파일 목록 | 서랍 열어보기 | `ls` → `보고서.md  사진.png` |
| `cd 폴더명` | 폴더 이동 | 다른 방으로 이동 | `cd Documents` |
| `cd ..` | 상위 폴더로 이동 | 한 층 위로 올라가기 | `cd ..` |
| `mkdir 폴더명` | 새 폴더 만들기 | 새 서랍 만들기 | `mkdir my-project` |
| `cat 파일명` | 파일 내용 보기 | 문서 펼쳐보기 | `cat CLAUDE.md` |

### 직접 해보기

화면을 좌우로 분할해서 연습하면 효과적입니다.

- **왼쪽**: 이 강의 자료 (브라우저 또는 에디터)
- **오른쪽**: 터미널

> 💡 macOS: 창을 화면 왼쪽/오른쪽 끝으로 드래그하면 자동 분할됩니다.
> Windows: `Win + ←` / `Win + →` 키로 창을 반반 배치할 수 있습니다.

터미널에서 아래 명령어를 하나씩 입력해보세요:

```bash
# 1. 현재 위치 확인
pwd

# 2. 파일 목록 보기
ls

# 3. 바탕화면(또는 홈)으로 이동
cd ~

# 4. 테스트 폴더 만들기
mkdir terminal-test

# 5. 만든 폴더로 이동
cd terminal-test

# 6. 현재 위치 다시 확인
pwd
```

마지막 `pwd`의 결과에 `terminal-test`가 보이면 성공입니다!

테스트가 끝나면 원래 위치로 돌아옵니다:

```bash
cd ~
```

---

## 0. Why Claude Code?

> ⏱ 5분

### AI 도구들은 비슷해지고 있다

ChatGPT에는 Codex가, Google에는 Gemini CLI가, Anthropic에는 Claude Code가 있습니다. 세 도구 모두 비슷한 방향으로 수렴하고 있습니다:

| | Claude Code | OpenAI Codex | Gemini CLI |
|---|---|---|---|
| 로컬 파일 접근 | ✅ | ✅ | ✅ |
| 터미널 실행 | ✅ | ✅ | ✅ |
| 자연어 대화 | ✅ | ✅ | ✅ |

**모델 성능도 엎치락뒤치락합니다.** 어떤 모델이 "최고"인지는 태스크마다, 시기마다 달라집니다.

### 그래서 왜 Claude Code인가?

차이는 **모델**이 아니라 **확장성**에 있습니다.

AI가 진짜 쓸모 있으려면 모델 밖 세상 — 슬랙, 지라, 노션, 캘린더 등 — 과 통신할 수 있어야 합니다. Claude Code는 이 확장성을 가장 빠르고 성숙하게 만들어가고 있습니다:

| 확장 수단 | 하는 일 | 예시 |
|-----------|--------|------|
| **MCP** (Model Context Protocol) | 외부 도구 연결을 위한 개방형 표준 | Slack, Jira, Notion, Gmail 등과 직접 통신 |
| **Skills** | 반복 워크플로우를 명령어로 저장 | `/briefing` → 이메일+슬랙 요약을 한 번에 |
| **Subagents** | 작업을 여러 AI에게 분업 | 코드 리뷰 + 테스트 + 문서화를 병렬 실행 |

이런 확장 기능 덕분에 이런 워크플로우가 가능합니다:

```
슬랙에서 이번 주 논의 수집 → 요약 → Notion에 정리 → Jira 이슈 생성
```

한 세션 안에서 여러 도구를 넘나들며 **업무 흐름 전체를 자동화**할 수 있습니다.

### 핵심 메시지

> **모델은 교체할 수 있지만, 워크플로우는 자산으로 남는다.**

우리가 Claude Code를 배우는 이유는 "가장 똑똑한 AI를 쓰기 위해"가 아닙니다. **나의 업무 도구들을 연결하고, 반복 작업을 자동화하는 워크플로우를 만들기 위해**서입니다.

오늘은 그 첫 단계로, Claude Code를 나에게 맞게 설정하는 법을 배웁니다.

---

## 1. Setup: 환경 준비

> ⏱ 15분

Claude Code를 사용하려면 몇 가지 도구를 설치해야 합니다. 터미널을 열고, 자신의 운영체제(macOS / Windows)에 맞는 가이드를 따라하세요.

### Step 1: GitHub 계정 만들기

GitHub는 코드와 파일을 저장하고 공유하는 플랫폼입니다. Claude Code가 프로젝트를 관리할 때 사용합니다.

1. https://github.com 접속
2. **"Sign up"** 클릭
3. 이메일 입력 → 비밀번호 설정 → 사용자명 입력
4. 이메일로 온 인증 코드 입력
5. 가입 완료

> 이미 계정이 있으면 이 단계를 건너뛰세요.

**확인:**

브라우저에서 https://github.com 에 로그인이 되면 성공입니다.

### Step 2: Git 설치

Git은 파일의 변경 이력을 추적하는 도구입니다. Claude Code가 파일을 수정할 때 **되돌리기가 가능**하도록 해주는 안전장치입니다.

#### 이미 설치되어 있는지 확인

```bash
git --version
```

`git version 2.x.x` 같은 출력이 나오면 → **이미 설치됨. Step 3의 나머지를 건너뛰세요.**

`command not found` 또는 에러가 나오면 → 아래 설치를 진행하세요.

#### macOS 설치

```bash
# Homebrew가 없으면 먼저 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Git 설치
brew install git
```

#### Windows 설치

1. https://git-scm.com/download/win 접속
2. **"Click here to download"** 클릭하여 설치 파일 다운로드
3. 다운로드된 파일 실행 → **모든 옵션을 기본값 그대로** 두고 "Next" 클릭
4. 설치 완료 후 **터미널을 닫았다가 다시 열기**

#### 설치 확인

```bash
git --version
```

```
# 이렇게 나오면 성공:
git version 2.47.1
```

#### 기본 설정

Git에 내 이름과 이메일을 등록합니다 (따옴표 안의 내용을 본인 정보로 바꾸세요):

```bash
git config --global user.name "홍길동"
git config --global user.email "gildong@example.com"
```

**확인:**

```bash
git config --global user.name
git config --global user.email
```

```
# 이렇게 나오면 성공:
홍길동
gildong@example.com
```

### Step 3: GitHub CLI 설치

GitHub CLI(`gh`)는 터미널에서 GitHub을 사용할 수 있게 해주는 도구입니다. Claude Code가 PR 생성, 이슈 관리 등을 할 때 사용합니다.

#### 이미 설치되어 있는지 확인

```bash
gh --version
```

`gh version 2.x.x` 같은 출력이 나오면 → **이미 설치됨. "GitHub 로그인"으로 건너뛰세요.**

#### macOS 설치

```bash
brew install gh
```

#### Windows 설치

1. https://cli.github.com 접속
2. **"Download for Windows"** 클릭
3. 다운로드된 파일 실행 → 안내에 따라 설치
4. 설치 완료 후 **터미널을 닫았다가 다시 열기**

#### 설치 확인

```bash
gh --version
```

```
# 이렇게 나오면 성공:
gh version 2.67.0 (2025-01-31)
```

#### GitHub 로그인

```bash
gh auth login
```

대화형 안내가 나옵니다. 아래와 같이 선택하세요:

```
? Where do you use GitHub? → GitHub.com
? What is your preferred protocol for Git operations on this host? → HTTPS
? Authenticate Git with your GitHub credentials? → Yes
? How would you like to authenticate GitHub CLI? → Login with a web browser
```

브라우저가 열리면 **"Authorize"** 버튼을 클릭하여 인증을 완료합니다.

**확인:**

```bash
gh auth status
```

```
# 이렇게 나오면 성공:
github.com
  ✓ Logged in to github.com account 내사용자명
```

### Step 4: Claude Code 설치 확인

```bash
claude --version
```

`claude x.x.x` 같은 출력이 나오면 → **이미 설치됨. 건너뛰세요.**

설치가 필요하면:

```bash
npm install -g @anthropic-ai/claude-code
```

> `npm`이 없다는 에러가 나오면, 먼저 Node.js를 설치해야 합니다:
> - **macOS**: `brew install node`
> - **Windows**: https://nodejs.org 에서 LTS 버전 다운로드 후 설치

**확인:**

```bash
claude --version
```

```
# 이렇게 나오면 성공:
claude 1.0.38
```

### 최종 체크리스트

세 명령어를 모두 실행해서 버전이 출력되는지 확인하세요:

```bash
git --version     # ✅ Git
gh --version      # ✅ GitHub CLI
claude --version  # ✅ Claude Code
```

모두 성공하면 다음 단계로 넘어갑니다.

---

## 2. CLAUDE.md: 나만의 매뉴얼

> ⏱ 25분

### CLAUDE.md란?

새 팀원이 들어오면 업무 가이드를 주죠? CLAUDE.md는 **Claude에게 주는 업무 가이드**입니다.

Claude Code는 매번 새로운 대화를 시작할 때 CLAUDE.md를 읽습니다. 여기에 적어둔 지시사항을 따릅니다.

예를 들어, CLAUDE.md에 이렇게 적으면:

```markdown
- 항상 한국어로 답변해주세요
- 존댓말을 사용해주세요
```

Claude는 매 대화마다 이 규칙을 읽고 따릅니다.

### Scope: CLAUDE.md를 어디에 두느냐에 따라 적용 범위가 달라진다

CLAUDE.md의 핵심은 **어디에 놓느냐**입니다. 위치에 따라 적용 범위(scope)가 달라집니다.

```
🌍 글로벌 (~/.claude/CLAUDE.md)
 └─ 모든 프로젝트에 적용
     예: "항상 한국어로 답변해줘", "존댓말 사용"

📁 프로젝트 (프로젝트 폴더/CLAUDE.md)
 └─ 이 프로젝트에서만 적용
     예: "이 프로젝트는 여행 관련 문서야", "마크다운으로 작성해"

📂 하위폴더 (하위폴더/CLAUDE.md)
 └─ 그 폴더 안에서만 적용
     예: "이 폴더의 파일은 보고서 형식을 따라"
```

비유로 이해하면 쉽습니다:

| Scope | 비유 | 예시 |
|-------|------|------|
| 🌍 글로벌 | 회사 전체 규칙 (복장 규정) | "한국어로 답변", "존댓말 사용" |
| 📁 프로젝트 | 부서 규칙 (마케팅팀 보고서 양식) | "이 프로젝트는 여행 사업" |
| 📂 하위폴더 | 특정 업무 규칙 (캠페인 톤앤매너) | "이 폴더는 보고서 형식" |

### 팀 공유 vs 개인용 — 어떻게 구분하나요?

CLAUDE.md는 용도에 따라 **팀과 공유하는 것**과 **나만 쓰는 것**으로 나뉩니다:

| 파일 | 공유 여부 | 언제 쓸까? |
|------|----------|-----------|
| `./CLAUDE.md` | **팀 공유** (git에 올라감) | 프로젝트 규칙, 팀 컨벤션 |
| `./CLAUDE.local.md` | **개인용** (git에 안 올라감) | 내 개인 설정, 테스트 URL |
| `~/.claude/CLAUDE.md` | **개인용** (홈 디렉토리) | 전역 개인 선호 (언어, 스타일) |

**핵심 원칙:**
- 팀과 공유하고 싶으면 → `./CLAUDE.md`에 쓰고 git 커밋
- 나만 쓸 거면 → `./CLAUDE.local.md` (이 프로젝트만) 또는 `~/.claude/CLAUDE.md` (모든 프로젝트)

### 유용한 명령어

| 명령어 | 설명 |
|--------|------|
| `/init` | 프로젝트를 분석해서 CLAUDE.md를 자동 생성 |
| `#` + 지시사항 | 빠르게 CLAUDE.md에 규칙 추가 (예: `# 한국어로 답변해줘`) |
| `/memory` | 현재 로딩된 모든 CLAUDE.md 파일 확인 |

### 퀴즈 2

> **Q1. 모든 프로젝트에서 Claude가 한국어로 답하게 하려면 CLAUDE.md를 어디에 둘까요?**
>
> A) 프로젝트 폴더 안
> B) `~/.claude/CLAUDE.md`
> C) 아무 데나

<details>
<summary>정답 보기</summary>

**B) `~/.claude/CLAUDE.md`**

글로벌 CLAUDE.md는 모든 프로젝트에 적용됩니다. 프로젝트 폴더 안에 두면 그 프로젝트에서만 적용돼요.

</details>

> **Q2. 팀원은 모르게 내 개인 설정만 적용하고 싶으면?**
>
> A) `./CLAUDE.md`
> B) `./CLAUDE.local.md`
> C) 하위폴더에 CLAUDE.md 생성

<details>
<summary>정답 보기</summary>

**B) `./CLAUDE.local.md`**

`CLAUDE.local.md`는 git에 올라가지 않아서 나만 사용하는 설정을 넣기에 적합합니다.

</details>

### 실습: 나만의 CLAUDE.md 작성하기

> 💡 실습 템플릿: [exercises/my-first-claude-md.md](exercises/my-first-claude-md.md)

#### Step 1: 프로젝트 CLAUDE.md 만들어보기

먼저 현재 프로젝트에 CLAUDE.md를 만들어서 효과를 체험합니다.

실습용 프로젝트 폴더를 만들고 Claude Code를 실행하세요:

```bash
# 실습용 폴더 만들고 Git 초기화
mkdir my-first-project
cd my-first-project
git init
```

CLAUDE.md를 작성합니다:

```bash
cat > CLAUDE.md << 'EOF'
# 프로젝트 가이드

이 프로젝트는 학습용 프로젝트입니다.

## 규칙
- 파일은 마크다운(.md) 형식으로 작성
- 제목은 반드시 포함할 것
- 모든 답변은 반말로 해줘
EOF
```

Claude Code를 실행하고 질문해보세요:

```bash
claude
```

```
이 프로젝트에 대해 설명해줘
```

Claude가 CLAUDE.md의 규칙대로 **반말로** 답하는지 확인하세요!

확인했으면 Claude Code를 종료합니다 (`/exit` 또는 `Ctrl+C`).

#### Step 2: 글로벌 CLAUDE.md 추가하기

이제 모든 프로젝트에 적용되는 글로벌 CLAUDE.md를 만듭니다:

```bash
# 글로벌 CLAUDE.md 만들기 (이미 있으면 편집)
mkdir -p ~/.claude
cat > ~/.claude/CLAUDE.md << 'EOF'
# 나의 선호 설정

- 항상 한국어로 답변해주세요
- 존댓말을 사용해주세요
- 설명할 때 비유를 들어주세요
EOF
```

다시 Claude Code를 실행합니다:

```bash
claude
```

`/memory` 명령어로 어떤 CLAUDE.md 파일이 로딩되었는지 확인하세요:

```
/memory
```

**로딩된 CLAUDE.md 목록**이 표시됩니다. 다음 두 파일이 보이면 성공입니다:

- `~/.claude/CLAUDE.md` — 글로벌
- `./CLAUDE.md` — 프로젝트

> 글로벌에서는 "존댓말을 사용해주세요", 프로젝트에서는 "반말로 해줘"라고 지시했습니다.
> 같은 규칙이 충돌하면 **프로젝트 CLAUDE.md가 우선**합니다. 질문해서 확인해보세요!

확인했으면 Claude Code를 종료합니다.

#### Step 3: 하위폴더 CLAUDE.md로 Scope 체험하기

이번에는 하위폴더에도 CLAUDE.md를 만들어서 3단계 Scope를 직접 확인합니다:

```bash
# 하위폴더 만들기
mkdir reports

# 하위폴더 CLAUDE.md 작성
cat > reports/CLAUDE.md << 'EOF'
# 보고서 폴더 규칙

- 이 폴더의 파일은 보고서 형식을 따를 것
- 답변 시 "보고서 작성 모드입니다"라고 먼저 말할 것
EOF
```

Claude Code를 실행하고 `/memory`로 확인합니다:

```bash
claude
```

```
/memory
```

이제 **3개의 CLAUDE.md**가 로딩된 것을 확인할 수 있습니다:

- `~/.claude/CLAUDE.md` — 🌍 글로벌
- `./CLAUDE.md` — 📁 프로젝트
- `./reports/CLAUDE.md` — 📂 하위폴더

하위폴더의 규칙이 적용되는지 테스트해보세요:

```
reports 폴더에 월간 보고서를 작성해줘
```

Claude가 "보고서 작성 모드입니다"라고 말하면서 보고서를 작성하는지 확인하세요!

#### Step 4: git으로 기록하기

```bash
git add CLAUDE.md reports/CLAUDE.md
git commit -m "프로젝트 CLAUDE.md 추가"
```

---

## 3. Memory: 기억하는 Claude

> ⏱ 15분

### Memory란?

CLAUDE.md가 **내가 쓰는 매뉴얼**이라면, Memory는 **Claude가 스스로 쓰는 업무 일지**입니다.

대화하면서 Claude가 유용하다고 판단한 정보를 자동으로 저장합니다. 다음 대화에서 이 정보를 기억하고 활용합니다.

예를 들어:
- "내 이름은 김철수야" → Claude가 Memory에 저장
- 다음 대화에서 "내 이름이 뭐였지?" → "김철수님이시죠"

> Memory는 2026년 2월에 출시된 최신 기능(Auto Memory)입니다.

### CLAUDE.md vs Memory — 뭐가 다른데?

| | **CLAUDE.md** | **Memory** |
|--|---------------|------------|
| **누가 작성?** | 내가 직접 ✍️ | Claude가 자동 🤖 |
| **성격** | 지시·규칙 | 학습·메모 |
| **비유** | 업무 매뉴얼 | 업무 일지 |
| **Scope** | 3단계 (글로벌/프로젝트/하위폴더) | 프로젝트 단위 |
| **로딩** | 매 세션, 전체 로딩 | 매 세션, 첫 200줄 |
| **수정** | 파일 직접 편집, `#` 단축키 | "기억해줘"/"잊어줘", `/memory` |
| **공유** | `./CLAUDE.md`는 팀 공유 가능 | 항상 개인 |
| **저장 위치** | 프로젝트 폴더 또는 홈 디렉토리 | `~/.claude/projects/<프로젝트>/memory/` |

핵심 차이를 한 줄로 요약하면:

- **CLAUDE.md** = 내가 Claude에게 주는 **규칙** → _"이렇게 해"_
- **Memory** = Claude가 스스로 쌓는 **학습** → _"아, 이렇게 하시는구나"_

### Memory는 어디에 저장되나요?

```
~/.claude/projects/<프로젝트>/memory/
├── MEMORY.md          ← 메인 파일 (첫 200줄 자동 로딩)
├── debugging.md       ← 토픽별 상세 노트 (필요 시 참조)
├── patterns.md        ← Claude가 필요에 따라 생성
└── ...
```

- `MEMORY.md`의 첫 200줄만 매 대화 시작 시 자동 로딩
- 나머지 토픽 파일은 Claude가 필요할 때 읽음
- 프로젝트(git repo) 단위로 관리됨

### Memory 관련 명령어

| 방법 | 설명 |
|------|------|
| "이걸 기억해줘" | 대화 중 Claude에게 직접 요청 |
| "이건 잊어줘" | 저장된 Memory 삭제 요청 |
| `/memory` | Memory 파일 목록 확인, 자동 저장 on/off 토글 |

### 퀴즈 3

> **Q. 다음 중 CLAUDE.md에 넣을 것(📋)과 Memory에 맡길 것(🧠)을 구분하세요:**
>
> 1. "항상 존댓말 사용"
> 2. "이 프로젝트의 빌드 명령어는 npm run build"
> 3. "보고서는 A4 형식으로 작성"
> 4. "지난번 회의에서 디자인 변경이 결정됨"

<details>
<summary>정답 보기</summary>

1. 📋 CLAUDE.md — 규칙/지시사항
2. 🧠 Memory — Claude가 학습할 프로젝트 사실
3. 📋 CLAUDE.md — 규칙/지시사항
4. 🧠 Memory — 맥락 정보

**기준**: 내가 정한 규칙이면 CLAUDE.md, Claude가 알아두면 좋은 사실/맥락이면 Memory

</details>

### 실습: Memory 체험하기

#### Step 1: Claude에게 기억 요청하기

Claude Code를 실행하고:

```
내 이름은 ○○○이야. 기억해줘.
```

Claude가 "Writing memory" 표시와 함께 저장하는 것을 확인하세요.

#### Step 2: 새 대화에서 확인하기

Claude Code를 종료하고 다시 시작합니다:

```bash
# 종료: Ctrl+C 또는 /exit
# 다시 시작
claude
```

```
내 이름이 뭐였지?
```

Claude가 기억하고 있다면 성공!

#### Step 3: Memory 파일 직접 확인하기

```bash
# Memory 파일 위치 확인
ls ~/.claude/projects/
```

`/memory` 명령어로 Memory 파일 목록을 확인할 수도 있습니다:

```
/memory
```

> 💡 Memory 파일은 일반 마크다운이라 직접 열어서 편집하거나 삭제할 수도 있습니다.

---

## 4. Wrap-up: 오늘 배운 것

> ⏱ 5분

오늘 배운 2가지를 정리합니다:

| 개념 | 비유 | 한 줄 요약 |
|------|------|-----------|
| **CLAUDE.md** | 업무 매뉴얼 | 내가 쓰는 규칙. 위치(scope)에 따라 적용 범위가 달라짐 |
| **Memory** | 업무 일지 | Claude가 자동으로 쌓는 학습. 항상 개인용 |

### 종합 퀴즈

> **Q1. Claude가 매번 영어로 답하는데, 어떻게 고칠까요?**

<details>
<summary>정답 보기</summary>

`~/.claude/CLAUDE.md`에 "한국어로 답변해주세요"를 추가합니다. 글로벌 CLAUDE.md는 모든 프로젝트에 적용됩니다.

</details>

> **Q2. 팀원과 프로젝트 규칙을 공유하고 싶으면?**

<details>
<summary>정답 보기</summary>

프로젝트 루트의 `./CLAUDE.md`에 작성하고 `git commit`합니다. 이 파일은 git을 통해 팀원과 공유됩니다.

</details>

> **Q3. Claude가 전에 알려준 정보를 다음에도 기억하게 하려면?**

<details>
<summary>정답 보기</summary>

"기억해줘"라고 말하면 됩니다. Claude가 Memory에 자동 저장합니다. 또는 Auto Memory가 켜져 있으면 Claude가 유용한 정보를 알아서 저장하기도 합니다.

</details>

---

## 다음 단계

오늘 설정한 것들을 실제 업무에 활용해보세요:

- [ ] 나의 글로벌 CLAUDE.md를 업무 스타일에 맞게 다듬기
- [ ] 실제 프로젝트에 CLAUDE.md 추가해보기
- [ ] Claude와 대화하면서 Memory가 쌓이는 것 관찰하기

> 📚 더 알고 싶으면?
> - [LLM 기초 개념](references/llm-basics.md) — Context Window, System Prompt란?
> - [Git 치트시트](references/git-cheatsheet.md) — 더 많은 Git 명령어
> - [CLAUDE.md 심화](references/claude-md-advanced.md) — @import, rules 등 고급 기능
> - [공식 문서](https://code.claude.com/docs/en/memory) — Claude Code Memory 공식 가이드
