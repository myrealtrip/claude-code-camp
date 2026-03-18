# Day 2: 나만의 Skill 만들기 — 도구에서 워크플로우까지

> 반복 업무를 `/명령어` 하나로 자동화하는 법을 배웁니다.
> 소요 시간: 약 60분 | 난이도: 입문 | 전제: Day 1 이수

### 대상

- 개발자, 비개발 직군(마케터, 세일즈, 전략, 재무, 운영 등) 모두 포함
- Day 1을 이수하고 CLAUDE.md, Memory 개념을 이해한 분

### 오늘 다루는 내용

| 순서 | 주제 | 한 줄 요약 |
|------|------|-----------|
| 0 | Recap & 오늘의 여정 | Day 1 복습 + 3단계 빌딩 블록 소개 |
| 1 | 도구 써보기 | Claude가 쓸 수 있는 도구를 하나씩 직접 체험 |
| 2 | 도구 → Skill | 체험한 도구들을 하나의 Skill로 엮기 |
| 3 | Skill → Skill 체이닝 | Skill이 다른 Skill을 부르는 자동화 |
| 4 | 실전 Skill 구상 | 본인 업무에 맞는 Skill 만들기 |
| 5 | Wrap-up | 정리 및 퀴즈 |

---

## 목차

- [0. Recap & 오늘의 여정](#0-recap--오늘의-여정)
- [1. 도구 써보기](#1-도구-써보기)
- [2. 도구 → Skill](#2-도구--skill)
- [3. Skill → Skill 체이닝](#3-skill--skill-체이닝)
- [4. 실전 Skill 구상](#4-실전-skill-구상)
- [5. Wrap-up](#5-wrap-up)

---

## 준비물: 시작하기 전에

### Day 1 완료 확인

- [x] Claude Code 설치 완료
- [x] Git, GitHub CLI 설치 완료
- [x] CLAUDE.md, Memory 개념 이해

### skill-creator 플러그인 설치

Claude Code를 실행한 뒤 아래 명령어를 입력하세요:

```
/plugin install skill-creator@claude-plugins-official
```

설치 확인:

```
/plugin
```

→ Installed 탭에서 `skill-creator`가 보이면 성공!

### 실습 프로젝트 준비

Day 1에서 만든 `my-first-project` 폴더를 사용합니다. 없으면 새로 만드세요:

```bash
mkdir my-first-project
cd my-first-project
git init
```

---

## 0. Recap & 오늘의 여정

> ⏱ 3분

### Day 1 복습 (30초)

| 개념 | 비유 | 한 줄 요약 |
|------|------|-----------|
| **CLAUDE.md** | 업무 매뉴얼 | 내가 쓰는 규칙. Claude가 매번 읽고 따름 |
| **Memory** | 업무 일지 | Claude가 자동으로 쌓는 학습 |

### 오늘의 여정: 3단계 빌딩 블록

> Day 1에서 Claude의 **성격**을 만들었다면, 오늘은 Claude에게 **기술**을 가르칩니다.

```
🧱 도구(Tool)        →  🏗️ Skill           →  🏙️ Skill 체이닝
하나의 동작            여러 도구를 조합한       Skill끼리 연결한
                      워크플로우              자동화 파이프라인

"망치질 한 번"         "선반 조립 설명서"      "가구 세트 완성"
```

| 단계 | 비유 | 배우는 것 |
|------|------|----------|
| 🧱 도구 | 망치, 톱, 자 | Claude가 쓸 수 있는 도구를 하나씩 체험 |
| 🏗️ Skill | 조립 설명서 | 도구들을 엮어서 워크플로우 만들기 |
| 🏙️ 체이닝 | 가구 세트 | Skill이 다른 Skill을 부르는 자동화 |

---

## 1. 도구 써보기

> ⏱ 15분

> "Skill을 만들기 전에, 재료부터 만져봅시다."

Claude Code를 실행하세요:

```bash
cd ~/my-first-project
claude
```

### 도구 1: AskUserQuestion (3분)

**이게 뭐야?**
- Claude가 사용자에게 **질문**하는 도구
- 비유: 접수 창구에서 "어떤 용건이신가요?" 물어보는 것
- 이게 없으면 Claude가 혼자 독백만 함

**직접 해보기:**

```
나한테 오늘 기분이 어떤지 물어보고, 답변에 맞는 노래를 추천해줘
```

- Claude가 기분을 **물어보는지** 확인
- "일방적으로 말하지 않고 **물어보고 → 반응하는** 패턴"

### 도구 2: Read + Write (4분)

**이게 뭐야?**
- Read = 파일 읽기, Write = 파일 만들기
- 비유: 서류 꺼내 읽기 / 새 서류 작성

**직접 해보기:**

```
CLAUDE.md 파일을 읽어서 내용을 요약한 뒤, summary.md 파일로 저장해줘
```

- Claude가 Read로 파일을 읽고 → Write로 새 파일을 만드는 과정 확인

### 도구 3: Bash (3분)

**이게 뭐야?**
- 터미널 명령을 실행하는 도구
- 비유: 비서에게 심부름 시키기

**직접 해보기:**

```
오늘 날짜와 요일을 확인하고, 이번 달 남은 날수를 계산해줘
```

- Claude가 `date` 명령을 실행하는 것을 확인
- "Claude가 직접 컴퓨터를 조작한다"

### 도구 4: WebSearch (3분)

**이게 뭐야?**
- 인터넷 검색 도구
- 비유: 구글 검색을 대신 해주는 것

**직접 해보기:**

```
최근 제주도 인기 관광지 트렌드를 검색해서 알려줘
```

- Claude가 검색하고 결과를 정리하는 것을 확인

### 중간 정리

**퀴즈: 방금 써본 도구 4개를 연결하세요**

| 하고 싶은 일 | 도구 |
|-------------|------|
| 사용자에게 물어보기 | ? |
| 파일 읽기/만들기 | ? |
| 터미널 명령 실행 | ? |
| 인터넷 검색 | ? |

<details>
<summary>정답 보기</summary>

| 하고 싶은 일 | 도구 |
|-------------|------|
| 사용자에게 물어보기 | **AskUserQuestion** |
| 파일 읽기/만들기 | **Read / Write** |
| 터미널 명령 실행 | **Bash** |
| 인터넷 검색 | **WebSearch** |

</details>

Claude Code를 종료합니다:

```
/exit
```

---

## 2. 도구 → Skill

> ⏱ 17분

> "도구를 하나씩 써봤습니다. 이제 이것들을 **하나의 레시피로 묶어봅시다**."

### 2-1. Skill이란? (2분)

- 방금 4개 도구를 **매번 말로 시키면** 번거로움
- Skill = 여러 도구를 **순서대로 쓰는 지시사항을 파일로 저장**한 것
- 비유: 요리할 때 재료(도구)를 매번 고민하지 않고, **레시피(Skill)를 따라하면 되는 것**

### 2-2. Skill 파일 구조 (3분)

> 📚 공식 문서: https://code.claude.com/docs/en/skills

```
프로젝트/
└── .claude/
    └── skills/                     ← 스킬 폴더
        └── weekly-report/          ← 스킬 이름 = 디렉토리 이름
            ├── SKILL.md            ← 메인 지시사항 (필수)
            └── (참조 파일들)        ← 선택
```

- **디렉토리 이름** = `/명령어` 이름 (`weekly-report/` → `/weekly-report`)
- **SKILL.md** = Claude에게 주는 지시사항 (필수, 이 파일만 있으면 동작)
- 참조 파일 = 템플릿, 예시 등 (선택, 복잡한 Skill에서 활용)

**SKILL.md 안의 구조는 3가지뿐:**

```yaml
---
description: 한 줄 설명              ← ① 이 Skill이 뭔지
---

# 제목                               ← ② 역할 정의
너는 ~를 하는 도우미입니다.

## 진행 방식                          ← ③ 단계별 지시사항
1. AskUserQuestion으로 ~를 물어본다
2. Bash로 날짜를 확인한다
3. Write로 파일을 저장한다
```

**Skill 저장 위치에 따른 적용 범위:**

| 위치 | 경로 | 적용 범위 |
|------|------|----------|
| 개인용 | `~/.claude/skills/<이름>/SKILL.md` | 내 모든 프로젝트 |
| 프로젝트 | `.claude/skills/<이름>/SKILL.md` | 이 프로젝트만 |

**Frontmatter 주요 옵션:**

| 필드 | 필수 | 설명 |
|------|------|------|
| `name` | 아니오 | 스킬 이름. 생략하면 디렉토리 이름 사용 |
| `description` | 권장 | 스킬 설명. Claude가 자동 판단에 사용 |
| `disable-model-invocation` | 아니오 | `true`면 `/명령어`로만 실행 (Claude가 자동 실행 안 함) |
| `allowed-tools` | 아니오 | 사용 가능한 도구 제한 |

**Day 1의 CLAUDE.md와 비교:**

| | CLAUDE.md | SKILL.md |
|--|-----------|----------|
| 위치 | 프로젝트 루트 | `.claude/skills/이름/` 안 |
| 실행 | 항상 자동 | `/명령어`로 실행 |
| 역할 | "넌 이런 사람이야" | "이 일은 이렇게 해" |

### 2-3. 직접 만들어보기: 할 일 정리 Skill (5분)

> 섹션 1에서 써본 **AskUserQuestion**을 Skill에 넣어봅니다.

**① 만들기**

```bash
mkdir -p .claude/skills/todo
```

아래를 통째로 복사해서 붙여넣기:

```bash
cat > .claude/skills/todo/SKILL.md << 'EOF'
---
description: 오늘의 할 일을 정리해줍니다
---

# 오늘의 할 일 정리

사용자의 할 일을 정리해주는 도우미입니다.

## 진행 방식

1. AskUserQuestion으로 "오늘 해야 할 일을 모두 알려주세요 (쉼표로 구분)"를 물어봅니다
2. 입력받은 항목을 아래 기준으로 분류합니다:
   - 🔴 긴급하고 중요한 일
   - 🟡 중요하지만 급하지 않은 일
   - 🟢 간단히 처리할 일
3. 분류 결과를 마크다운 체크리스트로 보여줍니다
4. AskUserQuestion으로 "수정할 부분이 있나요?"를 확인합니다
EOF
```

**② 실행해보기**

```bash
claude
```

```
/todo
```

- Skill이 질문을 하는지 확인 → "이것이 AskUserQuestion의 효과"
- 확인 후 Claude Code 종료

**③ 되돌아보기**

- 마크다운 파일 하나 만들었을 뿐인데
- `/todo` 명령어가 생겼고
- Claude가 지시사항대로 질문하고 → 분류하고 → 정리해줬다
- **"코딩 한 줄 없이, 글만 써서 자동화를 만든 것"**

### 2-4. skill-creator로 실전 Skill 만들기 (7분)

> 수동으로 원리를 이해했으니, 이제 **공식 도구**를 사용합니다.

**skill-creator란?**

- Skill을 만들어주는 Skill (메타!)
- 공식 플러그인: `skill-creator@claude-plugins-official`
- 대화로 원하는 것을 설명하면, 최적화된 SKILL.md를 자동 생성
- eval 테스트까지 가능하지만, 간단히 만들기도 가능

**실습: 주간 보고서 Skill 만들기**

```bash
claude
```

```
/skill-creator
```

Claude가 인터뷰를 시작하면 아래처럼 답하세요:

```
주간 보고서를 작성하는 Skill을 만들고 싶어.

- Bash로 오늘 날짜를 확인
- AskUserQuestion으로 이번 주에 한 일, 다음 주 계획, 특이사항을 물어봄
- 보고서 형식으로 정리해서 보여주고
- 최종본을 reports/weekly-{날짜}.md 파일로 저장

eval은 필요 없고, 바로 만들어줘.
```

- skill-creator가 `.claude/skills/weekly-report/SKILL.md`를 생성하는 것을 확인
- 생성 후 바로 `/weekly-report`로 실행하여 동작 확인
- 확인 후 Claude Code 종료

### 퀴즈

> **Q1. Skill과 CLAUDE.md의 차이는?**
>
> A) Skill은 `/명령어`로 실행, CLAUDE.md는 항상 자동 적용
> B) 둘 다 같은 것
> C) CLAUDE.md가 Skill보다 강력하다

<details>
<summary>정답 보기</summary>

**A) Skill은 `/명령어`로 실행, CLAUDE.md는 항상 자동 적용**

CLAUDE.md는 매 대화 시작 시 자동으로 로딩되어 항상 적용됩니다. Skill은 `/명령어`로 실행할 때만 작동합니다.

</details>

> **Q2. Skill 파일은 어디에 만들어야 하나요?**
>
> A) 프로젝트 루트에 `SKILL.md`
> B) `.claude/skills/<이름>/SKILL.md`
> C) `~/.claude/CLAUDE.md`

<details>
<summary>정답 보기</summary>

**B) `.claude/skills/<이름>/SKILL.md`**

Skill은 `.claude/skills/` 디렉토리 아래에 스킬 이름으로 된 폴더를 만들고, 그 안에 `SKILL.md` 파일을 넣어야 합니다.

</details>

---

## 3. Skill → Skill 체이닝

> ⏱ 15분

> "Skill 하나가 유용하면, **Skill끼리 연결하면** 더 강력해집니다."

### 3-1. 체이닝이란? (3분)

- 비유: 공장의 컨베이어 벨트 — 한 공정의 결과가 다음 공정의 입력이 됨
- 예시:

```
/weekly-report → 보고서 파일 생성
       ↓
/review → 보고서를 읽고 피드백
       ↓
최종 보고서 완성
```

- 방법: Skill의 **출력 파일을 다음 Skill의 입력**으로 사용하거나, **하나의 Skill 안에 여러 단계를 묶기**

### 3-2. 리뷰 Skill 만들기 (5분)

skill-creator로 리뷰 Skill을 만듭니다:

```bash
claude
```

```
/skill-creator
```

```
문서를 읽고 개선점을 제안하는 리뷰 Skill을 만들어줘.

- AskUserQuestion으로 리뷰할 파일 경로를 물어봄
- Read로 해당 파일을 읽음
- 빠진 내용, 표현 명확성, 구조 읽기 쉬움 관점에서 피드백
- AskUserQuestion으로 피드백 반영할지 물어봄
- 반영 원하면 Edit으로 원본 파일 직접 수정

eval 없이 바로 만들어줘. 스킬 이름은 review로.
```

생성 확인 후 Claude Code 종료.

### 3-3. 체이닝 체험: 보고서 → 리뷰 (3분)

```bash
claude
```

먼저 보고서를 만들고:
```
/weekly-report
```

이어서 리뷰 요청:
```
/review
```

- 리뷰 Skill이 방금 만든 보고서 파일을 읽고 피드백하는 것을 확인
- "앞 Skill의 결과물을 뒷 Skill이 이어받는" 흐름 체험

### 3-4. 풀 프로세스 Skill 만들기 (4분)

이제 두 단계를 하나로 묶는 체이닝 Skill을 만듭니다:

```
/skill-creator
```

```
주간 보고서 작성부터 리뷰, 최종 정리까지 한 번에 실행하는 Skill을 만들어줘.

Step 1 - 보고서 작성:
- Bash로 오늘 날짜 확인
- AskUserQuestion으로 이번 주 한 일, 다음 주 계획, 특이사항 물어봄
- 보고서 초안을 reports/weekly-{날짜}.md로 저장

Step 2 - 자동 리뷰:
- 방금 저장한 파일을 Read로 읽음
- 빠진 내용, 표현, 구조 관점에서 스스로 리뷰
- 리뷰 결과와 개선 제안을 보여줌

Step 3 - 최종 확인:
- AskUserQuestion으로 개선사항 반영할지 물어봄
- 반영 원하면 Edit으로 파일 수정
- 완료 안내

eval 없이 바로. 스킬 이름은 weekly-full.
```

실행:
```
/weekly-full
```

- 하나의 명령어로 **작성 → 리뷰 → 수정**이 자동으로 흘러가는 것을 체험
- **"이것이 체이닝의 힘 — 여러 단계를 한 번에"**

### 퀴즈

> **Q. 체이닝의 장점은?**
>
> A) Skill 파일이 줄어든다
> B) 여러 단계의 작업을 한 명령어로 자동 실행할 수 있다
> C) Claude가 더 똑똑해진다

<details>
<summary>정답 보기</summary>

**B) 여러 단계의 작업을 한 명령어로 자동 실행할 수 있다**

체이닝은 여러 Skill의 단계를 연결하여, 하나의 `/명령어`로 전체 워크플로우를 자동 실행하는 것입니다.

</details>

---

## 4. 실전 Skill 구상

> ⏱ 5분

### 도구 전체 조감

> "지금까지 AskUserQuestion, Read, Write, Bash를 써봤습니다.
> 사실 Skill에서 쓸 수 있는 도구는 훨씬 많습니다."

**기본 도구 — Claude Code에 항상 있음:**

| 도구 | 하는 일 | Skill 활용 예시 |
|------|---------|----------------|
| AskUserQuestion | 사용자에게 질문 | ✅ 써봤음 |
| Read | 파일 읽기 | ✅ 써봤음 |
| Write | 파일 만들기 | ✅ 써봤음 |
| Edit | 파일 일부 수정 | 기존 문서의 특정 부분만 고치기 |
| Bash | 터미널 명령 | ✅ 써봤음 |
| Glob | 파일 검색 | "reports 폴더에서 .md 파일 찾기" |
| Grep | 내용 검색 | "프로젝트에서 '매출' 언급된 파일 찾기" |
| WebSearch | 인터넷 검색 | ✅ 써봤음 |
| WebFetch | 웹페이지 읽기 | 특정 URL 내용 가져와서 요약 |
| Agent | 하위 작업자 | 여러 작업을 동시에 시키기 |

**MCP 도구 — 외부 서비스 연결 시 추가됨 (Day 3 예고):**

| 도구 | 하는 일 | Skill 활용 예시 |
|------|---------|----------------|
| Slack | 슬랙 읽기/보내기 | "이번 주 슬랙 논의 요약" |
| Gmail | 이메일 읽기/초안 | "안 읽은 메일 브리핑" |
| Notion | 노션 페이지 관리 | "회의록을 노션에 저장" |
| Calendar | 일정 관리 | "오늘 회의 알려줘" |
| Jira/Linear | 이슈 관리 | "이 내용으로 티켓 만들어줘" |

> Skill에 자연어로 "인터넷에서 검색해줘", "슬랙에서 찾아줘"라고 쓰면
> Claude가 알맞은 도구를 알아서 골라 씁니다.
> 도구 이름을 외울 필요는 없지만, **어떤 게 가능한지** 알아두면 상상력이 넓어집니다.

### 아이디어 메뉴판

| 직군 | 단일 Skill | 체이닝 아이디어 |
|------|-----------|----------------|
| 마케터 | `/campaign-copy` 카피 초안 | 초안 → 톤 검수 → 최종본 |
| 세일즈 | `/followup` 미팅 후 이메일 | 미팅 노트 정리 → 이메일 초안 → 리뷰 |
| PM | `/ticket` 이슈 초안 | 요구사항 정리 → 티켓 작성 → 우선순위 분류 |
| 운영 | `/cs-reply` CS 답변 | FAQ 검색 → 답변 초안 → 톤 맞춤 |
| 공통 | `/meeting-notes` 회의록 | 회의록 작성 → 액션 아이템 추출 → 담당자 배정 |

### 실습: 본인 업무 Skill 만들기

```
/skill-creator
```

어떤 Skill을 만들고 싶은지 자유롭게 설명하세요!

---

## 5. Wrap-up

> ⏱ 5분

### 오늘 배운 3단계

| 단계 | 비유 | 배운 것 |
|------|------|---------|
| 🧱 도구 | 망치, 톱, 자 | AskUserQuestion, Read/Write, Bash, WebSearch |
| 🏗️ Skill | 조립 설명서 | `.claude/skills/<이름>/SKILL.md`에 도구들을 엮기 |
| 🏙️ 체이닝 | 가구 세트 | Skill의 출력 → 다음 Skill의 입력으로 연결 |

### 3일간의 여정

```
Day 1: Claude의 성격을 설정했다        (CLAUDE.md, Memory)
Day 2: Claude에게 기술을 가르쳤다       (도구 → Skill → 체이닝) ← 오늘
Day 3: Claude의 능력을 확장한다         (MCP — 외부 도구 연결)
```

### 종합 퀴즈

> **Q1. 도구, Skill, 체이닝의 관계를 올바르게 설명한 것은?**
>
> A) 도구는 Skill 안에서만 쓸 수 있다
> B) 도구를 조합하면 Skill, Skill을 연결하면 체이닝
> C) 체이닝은 코딩을 해야만 가능하다

<details>
<summary>정답 보기</summary>

**B) 도구를 조합하면 Skill, Skill을 연결하면 체이닝**

도구(Tool)는 하나의 동작, Skill은 여러 도구를 엮은 워크플로우, 체이닝은 Skill끼리 연결한 자동화 파이프라인입니다.

</details>

> **Q2. Skill을 만드는 가장 효율적인 방법은?**
>
> A) SKILL.md를 처음부터 수동으로 작성
> B) `/skill-creator`로 대화하며 생성
> C) 다른 사람의 Skill을 복사

<details>
<summary>정답 보기</summary>

**B) `/skill-creator`로 대화하며 생성**

`/skill-creator`는 공식 플러그인으로, 대화로 원하는 것을 설명하면 최적화된 SKILL.md를 자동 생성해줍니다. 물론 구조를 이해하기 위해 수동 작성도 해봐야 합니다.

</details>

> **Q3. 다음 중 Skill의 `description` 필드의 역할은?**
>
> A) 사용자에게 보여주는 도움말
> B) Claude가 언제 이 Skill을 쓸지 판단하는 근거
> C) Skill의 실행 순서를 결정

<details>
<summary>정답 보기</summary>

**B) Claude가 언제 이 Skill을 쓸지 판단하는 근거**

`description`은 Claude가 사용자의 요청에 맞는 Skill을 자동으로 선택할 때 참고하는 설명입니다. 잘 작성하면 Claude가 적절한 상황에서 자동으로 Skill을 사용합니다.

</details>

### 다음 단계

- [ ] 만든 Skill을 실제 업무에서 한 번 써보기
- [ ] 불편한 점이 있으면 `/skill-creator`로 Skill 수정해보기
- [ ] "이것도 자동화할 수 있을까?" 목록 적어오기 (Day 3 준비)

> 📚 더 알고 싶으면?
> - [Skills 공식 문서](https://code.claude.com/docs/en/skills) — Skill 작성 가이드
> - [Plugins 공식 문서](https://code.claude.com/docs/en/plugins) — 플러그인 만들기/공유
> - [skill-creator 플러그인](https://code.claude.com/docs/en/discover-plugins) — Skill 자동 생성 도구
