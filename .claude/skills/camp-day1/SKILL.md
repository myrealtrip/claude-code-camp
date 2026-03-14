---
name: camp-day1
description: "Day 1: CLAUDE.md와 Memory로 나에게 맞는 Claude Code 세팅하기 - 인터랙티브 강의 진행. Use when starting day 1 training, teaching Claude Code basics, CLAUDE.md setup, or onboarding new users."
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

## 강의 시작

강의를 시작할 때:
1. 간단히 인사하고 강의 개요를 소개한다
2. **Bash 도구로 `uname -s`를 실행하여 사용자의 OS를 자동 감지**한다
   - `Darwin` → macOS (터미널 앱 사용)
   - `Linux` → Linux (터미널 앱 사용)
   - 그 외 → Windows (PowerShell 앱 사용)
   - 감지 결과를 사용자에게 알려주고, 이후 해당 OS에 맞는 안내만 제공한다
3. 준비물(터미널, 기본 명령어)이 갖춰져 있는지 확인한다
4. 이후 섹션 0부터 순서대로 진행한다

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

### 섹션 0.5: 터미널 기본 명령어 (5분)

**목표**: 실습에서 사용할 터미널 명령어를 미리 익힌다.

**진행 순서**:

1. 터미널이 무엇인지 간단히 설명한다
   - **비유**: 텍스트로 컴퓨터에 명령을 내리는 창. 마우스 대신 글자로 "이 폴더 열어", "이 파일 보여줘"라고 말하는 것
2. 실습에서 자주 쓰는 명령어 5개를 소개한다:

   | 명령어 | 하는 일 | 비유 |
   |--------|--------|------|
   | `pwd` | 현재 위치 확인 | "나 지금 어디야?" |
   | `ls` | 파일 목록 보기 | 서랍 열어보기 |
   | `cd 폴더명` | 폴더 이동 | 다른 방으로 이동 |
   | `mkdir 폴더명` | 새 폴더 만들기 | 새 서랍 만들기 |
   | `cat 파일명` | 파일 내용 보기 | 문서 펼쳐보기 |

3. 사용자에게 터미널을 열고 아래 명령어를 하나씩 따라해보라고 안내한다:

   ```bash
   pwd
   ls
   cd ~
   mkdir terminal-test
   cd terminal-test
   pwd
   ```

   - 마지막 `pwd` 결과에 `terminal-test`가 보이면 성공
   - 원래 위치로 돌아오기: `cd ~`

4. AskUserQuestion으로 "터미널 명령어 연습이 잘 되었나요?"를 확인한다

---

### 섹션 1: Setup - 환경 준비 (15분)

**목표**: Git, GitHub CLI, Claude Code가 모두 설치되고 동작하는 상태를 만든다.

**진행 순서**:

1. **GitHub 계정 확인** — AskUserQuestion으로 GitHub 계정이 있는지 물어보고, 없으면 https://github.com 가입을 안내한다

2. **도구 설치 상태 자동 체크** — Bash 도구로 아래 명령어를 실행하여 설치 여부를 한 번에 확인한다:
   ```bash
   echo "=== Git ===" && (git --version 2>/dev/null || echo "NOT INSTALLED") && echo "=== GitHub CLI ===" && (gh --version 2>/dev/null | head -1 || echo "NOT INSTALLED") && echo "=== Claude Code ===" && (claude --version 2>/dev/null || echo "NOT INSTALLED")
   ```
   결과를 분석하여 사용자에게 알려준다:
   - 모두 설치됨 → "모든 도구가 준비되었습니다!" 안내 후 다음 섹션으로
   - 일부 미설치 → 미설치된 도구만 아래 가이드로 안내

3. **Git 미설치 시** — OS에 맞는 설치 가이드 안내:
   - macOS: `brew install git`
   - Windows: https://git-scm.com/download/win 에서 다운로드
   - 설치 후 기본 설정 안내:
     ```
     git config --global user.name "이름"
     git config --global user.email "이메일"
     ```

4. **GitHub CLI 미설치 시** — OS에 맞는 설치 가이드 안내:
   - macOS: `brew install gh`
   - Windows: https://cli.github.com 에서 다운로드
   - 설치 후 `gh auth login`으로 로그인 안내 (GitHub.com → HTTPS → Yes → Login with a web browser)

5. **Claude Code 미설치 시** — OS에 맞는 설치 가이드 안내:
   - macOS: `brew install claude-code`
   - Windows: `npm install -g @anthropic-ai/claude-code` (npm이 없으면 https://nodejs.org 에서 Node.js 먼저 설치)

6. **설치 후 재확인** — 미설치 도구가 있었다면, 설치 후 Bash 도구로 다시 확인:
   ```bash
   git --version && gh --version && claude --version
   ```
   모두 버전이 출력되면 "모든 도구가 준비되었습니다!" 안내

7. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 2: CLAUDE.md - 나만의 매뉴얼 (25분)

**목표**: CLAUDE.md의 개념, Scope, 팀/개인 구분을 이해하고 직접 작성한다.

#### Step 1: 설명 (5분)

1. CLAUDE.md란 무엇인지 설명한다
   - **비유**: 새 팀원에게 주는 업무 가이드
   - Claude Code가 매 대화 시작 시 읽고 따르는 지시사항 파일
2. **Scope** 개념을 설명한다 — 위치에 따라 적용 범위가 달라짐
   - 🌍 글로벌 (`~/.claude/CLAUDE.md`): 모든 프로젝트 → 회사 전체 규칙 (복장 규정)
   - 📁 프로젝트 (`./CLAUDE.md`): 이 프로젝트만 → 부서 규칙 (마케팅팀 보고서 양식)
   - 📂 하위폴더 (`./폴더/CLAUDE.md`): 그 폴더만 → 특정 업무 규칙 (캠페인 톤앤매너)
3. **팀 공유 vs 개인용** 구분을 설명한다
   - `./CLAUDE.md`: 팀 공유 (git에 올라감)
   - `./CLAUDE.local.md`: 개인용 (git에 안 올라감)
   - `~/.claude/CLAUDE.md`: 글로벌 개인용
4. 유용한 명령어 소개: `/init`, `#` + 지시사항, `/memory`

#### Step 2: 직접 만들어서 해보기 (17분)

사용자에게 단계별로 직접 실행하도록 안내한다. 막히면 도와준다.

> 실습 시작 전에 "**새 터미널 창(또는 탭)을 하나 열어주세요**"라고 안내한다.
> - macOS: 터미널 앱 (권장: [iTerm2](https://iterm2.com/downloads.html) 또는 `brew install --cask iterm2`)
> - Linux: 터미널 앱
> - Windows: PowerShell 앱 (시작 메뉴에서 "PowerShell" 검색)
> 이 강의 자료를 보면서 동시에 터미널에서 실행할 수 있도록 화면을 나란히 배치하도록 권장한다.

각 실습은 **① 만들기 → ② 답변으로 확인 → ③ `/memory`로 확인** 패턴으로 진행한다.

**실습 2-1: 프로젝트 CLAUDE.md 만들기**

**① 만들기**

새 터미널에서 아래를 복사해서 붙여넣으라고 안내한다:

```bash
mkdir my-first-project
cd my-first-project
git init
```

이어서 아래 명령어 블록을 **`cat`부터 `EOF`까지 전체 복사**해서 붙여넣으라고 안내한다:

```bash
cat > CLAUDE.md << 'EOF'
# 프로젝트 가이드

이 프로젝트는 마이리얼트립 학습용 프로젝트입니다.

## 규칙
- 파일은 마크다운(.md) 형식으로 작성
- 제목은 반드시 포함할 것
- 모든 답변은 반말로 해줘
EOF
```

> 복사 범위를 명확히 안내: "`cat > CLAUDE.md << 'EOF'`" 줄부터 마지막 "`EOF`" 줄까지 통째로 복사.
> 붙여넣고 Enter를 누르면 파일이 생성된다.

파일이 잘 만들어졌는지 확인:
```bash
cat CLAUDE.md
```

**② 답변으로 확인**

같은 터미널에서 Claude Code를 실행하라고 안내한다:

```bash
claude
```

실행되면 아래 질문을 입력해보라고 한다:

```
마이리얼트립에 대해 설명해줘
```

- Claude가 CLAUDE.md의 규칙대로 **반말로** 답하는지 확인하게 한다
- 반말로 답하면 "CLAUDE.md가 잘 적용된 것"이라고 설명한다

**③ `/memory`로 확인**

Claude Code 안에서 `/memory`를 입력하라고 안내한다:
- `./CLAUDE.md` (프로젝트) 가 로딩된 것을 확인
- 확인 후 `/exit` 또는 `Ctrl+C`로 Claude Code를 종료

AskUserQuestion으로 "반말로 답하는 것을 확인했나요?"를 확인한다.

---

**실습 2-2: 글로벌 CLAUDE.md 추가하기**

**① 만들기**

아래 명령어 블록을 **통째로 복사**해서 붙여넣으라고 안내한다:

```bash
mkdir -p ~/.claude
cat > ~/.claude/CLAUDE.md << 'EOF'
# 나의 선호 설정

- 항상 한국어로 답변해주세요
- 답변하려는 내용을 인터넷에서 검색해서 관련 링크를 인용해주세요
EOF
```

> 복사 범위: "`mkdir -p ~/.claude`" 줄부터 마지막 "`EOF`" 줄까지 전체.

**② 답변으로 확인**

다시 `claude`를 실행하고 질문한다:

```
마이리얼트립에 대해 설명해줘
```

- 글로벌 CLAUDE.md의 규칙대로 **인터넷 검색 후 링크를 인용**하는지 확인하게 한다
- 프로젝트 CLAUDE.md의 규칙대로 **반말로** 답하는지도 함께 확인한다
- 두 CLAUDE.md의 규칙이 동시에 적용되는 것을 체험하게 한다

**③ `/memory`로 확인**

`/memory`를 입력해서 로딩된 CLAUDE.md 목록을 확인하라고 안내한다:
- `~/.claude/CLAUDE.md` — 🌍 글로벌 (한국어, 링크 인용)
- `./CLAUDE.md` — 📁 프로젝트 (반말)
- 2개가 모두 로딩되어 서로 다른 규칙이 **동시에 적용**되는 것을 설명한다
- 확인 후 Claude Code를 종료

AskUserQuestion으로 진행 상황을 확인한다.

---

**실습 2-3: 하위폴더 CLAUDE.md로 Scope 체험**

**① 만들기**

아래 명령어 블록을 **통째로 복사**해서 붙여넣으라고 안내한다:

```bash
mkdir reports
cat > reports/CLAUDE.md << 'EOF'
# 보고서 폴더 규칙

- 이 폴더의 파일은 보고서 형식을 따를 것
- 답변 시 "보고서 작성 모드입니다"라고 먼저 말할 것
EOF
```

**② 답변으로 확인**

`claude`를 실행하고 아래 질문을 입력하라고 안내한다:

```
reports 폴더에 월간 보고서를 작성해줘
```

- Claude가 "보고서 작성 모드입니다"라고 말하면서 시작하는지 확인한다
- reports 폴더에 작업을 요청하면, 해당 폴더의 CLAUDE.md가 지침으로 작용한다는 점을 설명한다

**③ `/memory`로 확인**

Claude Code를 종료한 뒤, **reports 폴더로 이동**해서 다시 실행하라고 안내한다:

```bash
cd reports
claude
```

> 하위폴더의 CLAUDE.md는 부모 디렉토리에서 `/memory`를 쳐도 보이지 않는다.
> 해당 폴더로 직접 이동해야 `/memory`에서 확인할 수 있다.

`/memory`로 로딩된 CLAUDE.md 목록을 확인한다:
- `~/.claude/CLAUDE.md` — 🌍 글로벌
- `../CLAUDE.md` — 📁 프로젝트 (부모 디렉토리)
- `./CLAUDE.md` — 📂 하위폴더 (현재 디렉토리)

3단계 Scope가 동시에 적용되는 것을 확인했으면 Claude Code를 종료하고, 프로젝트 루트로 돌아간다:

```bash
cd ..
```

---

**실습 2-4: git으로 기록**

```bash
git add CLAUDE.md reports/CLAUDE.md
git commit -m "프로젝트 CLAUDE.md 추가"
```

AskUserQuestion으로 실습 완료 여부를 확인한다.

#### Step 3: 퀴즈 (3분)

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

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 3: Memory - 기억하는 Claude (15분)

**목표**: Memory의 개념과 CLAUDE.md와의 차이를 이해하고, 직접 Memory를 사용해본다.

#### 개념 설명 (5분)

1. Memory란 무엇인지 설명한다
   - **비유**: CLAUDE.md가 "내가 쓰는 업무 매뉴얼"이라면, Memory는 "Claude가 스스로 쓰는 업무 일지"
   - 대화 중 Claude가 유용하다고 판단한 정보를 자동 저장
   - 다음 대화에서 기억하고 활용
2. **CLAUDE.md vs Memory 비교표**를 제시한다:

   | | CLAUDE.md | Memory |
   |--|-----------|--------|
   | 누가 작성? | 내가 직접 | Claude가 자동 |
   | 성격 | 지시/규칙 | 학습/메모 |
   | 비유 | 업무 매뉴얼 | 업무 일지 |
   | Scope | 3단계 (글로벌/프로젝트/하위폴더) | 프로젝트 단위 |
   | 공유 | 팀 공유 가능 | 항상 개인 |

3. Memory 저장 위치: `~/.claude/projects/<프로젝트>/memory/`
   - `MEMORY.md` 첫 200줄만 매 세션 자동 로딩
4. Memory 관련 명령어: "기억해줘", "잊어줘", `/memory`

#### 퀴즈 (3분)

AskUserQuestion으로 아래 퀴즈를 **하나씩** 출제한다:

**Q1. "항상 존댓말을 사용해줘"는 어디에 넣어야 할까요?**
- A) CLAUDE.md
- B) Memory
- 정답: **A** — 내가 정한 규칙/지시사항이므로 CLAUDE.md에 넣는다.

**Q2. "지난번 회의에서 디자인 변경이 결정됐어"는 어디에 넣어야 할까요?**
- A) CLAUDE.md
- B) Memory
- 정답: **B** — 사실/맥락 정보이므로 "기억해줘"로 Memory에 맡긴다.

정답 해설 시 기준을 설명한다:
- **CLAUDE.md** = 내가 정한 **규칙/지시사항** → "이렇게 해"
- **Memory** = Claude가 알아두면 좋은 **사실/맥락** → "이런 일이 있었어"

> 솔직히 CLAUDE.md와 Memory는 겹치는 부분이 꽤 있다. "한국어로 답해줘"를 CLAUDE.md에 쓸 수도 있고, "기억해줘"로 Memory에 넣을 수도 있다. 이런 식으로 **같은 결과를 여러 방법으로 달성할 수 있는 경우가 많다.** 처음에는 구분이 헷갈릴 수 있는데, 쓰다 보면 감이 온다. 대략적인 기준은:
> - 항상 지켜야 하는 규칙이면 → CLAUDE.md
> - 한번 알려주면 알아서 기억하면 좋을 것 → Memory
> - 애매하면? 어디에 넣든 동작은 한다. 편한 쪽으로!

#### 실습 (7분)

**Step 1: Memory 없이 질문하기 — "모르는 상태" 확인**

먼저 AskUserQuestion으로 사용자의 이름을 물어본다. (이후 실습에서 사용)

앞서 실습하던 프로젝트 폴더(`my-first-project`)에서 `claude`를 실행하라고 안내한다:

```bash
claude
```

실행되면 아래 질문을 입력해보라고 한다:

```
내 이름이 뭐야?
```

- Claude가 이름을 **모른다고 답하는 것**을 확인하게 한다
- "아직 Memory에 저장된 게 없어서 모르는 게 당연하다"고 설명한다

**Step 2: 기억시키기**

같은 대화에서 이어서 입력한다 (사용자의 실제 이름으로):

```
내 이름은 OOO이야. 기억해줘.
```

- Claude가 "Writing memory" 표시와 함께 저장하는 것을 확인하게 한다
- `Ctrl+O`를 눌러서 Memory에 어떤 내용이 저장되었는지 크게 펼쳐서 확인하라고 안내한다
- 확인 후 `/exit`으로 Claude Code를 종료

**Step 3: 새 대화에서 같은 질문하기 — "기억하는 상태" 확인**

다시 Claude Code를 실행한다:

```bash
claude
```

아까와 **동일한 질문**을 입력한다:

```
내 이름이 뭐야?
```

- 이번에는 Claude가 이름을 **기억하고 답하는 것**을 확인한다
- Step 1에서는 몰랐는데, Memory에 저장한 후에는 기억한다는 차이를 체험하게 한다
- "이것이 Memory의 핵심 — 대화가 바뀌어도 기억이 유지된다"고 설명한다

**Step 4: Memory 파일 직접 확인**

Claude Code 안에서 `/memory`를 입력하여 Memory 파일 목록을 확인하라고 안내한다.
- Memory 파일은 일반 마크다운이라 직접 편집/삭제도 가능함을 안내한다
- 확인 후 Claude Code를 종료

**Step 5: 다른 프로젝트에서는 기억 못하는 것 확인 — Memory는 프로젝트(git repo) 단위**

홈 디렉토리로 이동해서 Claude Code를 실행하라고 안내한다:

```bash
cd ~
claude
```

같은 질문을 입력한다:

```
내 이름이 뭐야?
```

- Claude가 이름을 **모른다고 답하는 것**을 확인하게 한다
- 아까 `my-first-project`에서는 기억했는데, 여기서는 모른다는 차이를 짚어준다
- "Memory는 프로젝트(git repo) 단위로 저장되기 때문에, 다른 프로젝트에서는 기억이 공유되지 않는다"고 설명한다
- 확인 후 Claude Code를 종료하고 실습 폴더로 돌아간다:

```bash
cd ~/my-first-project
```

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 4: Wrap-up - 오늘 배운 것 (5분)

**목표**: 학습 내용을 정리하고 종합 퀴즈로 확인한다.

**진행 순서**:

1. 오늘 배운 2가지를 정리한다:
   - **CLAUDE.md** = 업무 매뉴얼. 내가 쓰는 규칙. 위치(scope)에 따라 적용 범위가 달라짐
   - **Memory** = 업무 일지. Claude가 자동으로 쌓는 학습. 항상 개인용

2. AskUserQuestion으로 종합 퀴즈를 **하나씩** 출제한다:

**Q1. Claude가 매번 영어로 답하는데, 어떻게 고칠 수 있을까요? (복수 선택)**
- A) 프로젝트 CLAUDE.md에 한국어 규칙 추가
- B) 글로벌 CLAUDE.md에 한국어 규칙 추가
- C) "한국어로 답해줘. 기억해줘"라고 말하기
- multiSelect: true
- 정답: **A, B, C 모두 정답** — 셋 다 가능하다!
  - A: 해당 프로젝트에서만 적용
  - B: 모든 프로젝트에 적용 (가장 추천)
  - C: Memory에 저장되어 해당 프로젝트에서 기억
  - 상황에 따라 적합한 방법이 다르다는 점을 설명한다

**Q2. 팀원과 프로젝트 규칙을 공유하고 싶으면?**
- A) `./CLAUDE.md`에 작성하고 git commit
- B) `./CLAUDE.local.md`에 작성
- C) 슬랙에 공유
- 정답: **A** — `./CLAUDE.md`는 git에 올라가므로 팀원과 공유된다.

**Q3. Claude가 전에 알려준 정보를 다음에도 기억하게 하려면?**
- A) CLAUDE.md에 적어둔다
- B) "기억해줘"라고 말한다
- C) 매번 다시 알려준다
- 정답: **B** — "기억해줘"라고 하면 Claude가 Memory에 자동 저장한다.

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
- 강의 시작 시 `uname -s` 명령어를 Bash 도구로 실행하여 OS를 자동 감지하세요. 이후 해당 OS에 맞는 안내만 하세요.
- 설명할 때는 비유를 적극 활용하세요. 교안에 있는 비유를 우선 사용하세요.
- 강의 시작 시 간단한 인사와 함께 강의 개요를 소개하고, 준비물이 갖춰져 있는지 확인하세요.
