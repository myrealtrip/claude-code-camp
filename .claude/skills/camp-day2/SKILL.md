---
description: "Day 2: 나만의 Skill 만들기 — 도구에서 워크플로우까지 - 인터랙티브 강의 진행"
---

# Day 2: 나만의 Skill 만들기 — 도구에서 워크플로우까지

이 스킬은 "나만의 Skill 만들기" 강의를 인터랙티브하게 진행합니다.

## 대상

- 개발자, 비개발 직군(마케터, 세일즈, 전략, 재무, 운영 등) 모두 포함
- Day 1을 이수하고 CLAUDE.md, Memory 개념을 이해한 분

## 진행 방식

강의를 섹션별로 진행합니다. 각 섹션에서:

1. 핵심 개념을 설명합니다
2. 실습이 있으면 사용자와 함께 직접 실행합니다
3. 퀴즈가 있으면 사용자에게 문제를 내고 답을 확인합니다
4. 사용자가 준비되면 다음 섹션으로 넘어갑니다

총 소요 시간: 약 60분

---

## 강의 시작

`use camp-lecture-start`

위 공통 루틴 실행 후, 아래 Day 2 전용 추가 단계를 진행한다:
4. 오늘의 3단계 여정(도구 → Skill → 오케스트레이션)을 소개한다
5. 준비물(Day 1 이수, skill-creator 플러그인)이 갖춰져 있는지 확인한다
6. 이후 섹션 0부터 순서대로 진행한다

---

## 섹션별 진행 가이드

### 섹션 0: Recap & 오늘의 여정 (3분)

**목표**: Day 1을 복습하고 오늘의 학습 로드맵을 이해시킨다.

**진행 순서**:

1. Day 1 핵심 30초 복습:
   - CLAUDE.md = 업무 매뉴얼 (내가 쓰는 규칙)
   - Memory = 업무 일지 (Claude가 자동으로 쌓는 학습)
2. 오늘의 여정 다이어그램을 브라우저에 띄운다:
   > Bash 도구로 아래 OS별 명령어를 실행하여 여정 다이어그램을 브라우저에 띄운다. 다이어그램을 보여주면서 설명한다.
   > - macOS: `open .claude/skills/camp-day2/journey-diagram.html`
   > - Linux: `xdg-open .claude/skills/camp-day2/journey-diagram.html`
   > - Windows: `start .claude/skills/camp-day2/journey-diagram.html`
   >
   > 다이어그램의 "다음" 버튼을 누르면 단계별로 진행되는 점을 안내한다:
   > - **0. As-Is**: 지금은 사용자와 Claude Code만 있는 상태
   > - **1. 도구 발견**: Claude Code가 사용할 수 있는 도구(Native Tools, MCP)를 발견
   > - **2. 첫 Skill**: 도구를 묶어 Skill 하나를 만듦
   > - **3. Skill 확장**: Skill이 여러 개로 늘어남
   > - **4. 오케스트레이션**: Skill끼리 연결해서 자동화 파이프라인 완성
   >
   > "오늘 우리는 0번에서 시작해서 4번까지 갈 겁니다!" 라고 안내한다.

3. 오늘의 여정을 3단계 빌딩 블록으로 소개한다:

   ```
   🧱 도구(Tool)        →  🏗️ Skill           →  🏙️ Skill 오케스트레이션
   하나의 동작            여러 도구를 조합한       Skill끼리 연결한
                         워크플로우              자동화 파이프라인

   "망치질 한 번"         "선반 조립 설명서"      "가구 세트 완성"
   ```

4. 핵심 메시지 전달: **"Day 1에서 Claude의 성격을 만들었다면, 오늘은 Claude에게 기술을 가르칩니다."**
5. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 0.5: 준비물 확인 (3분)

**목표**: skill-creator 플러그인이 설치되어 있는지 확인한다.

**진행 순서**:

1. AskUserQuestion으로 "Day 1에서 만든 `my-first-project` 폴더가 있나요?"를 확인한다
   - 없으면 아래 명령어를 새 터미널에서 실행하라고 안내한다:
     ```bash
     mkdir my-first-project
     cd my-first-project
     git init
     ```
2. skill-creator 플러그인 설치를 안내한다:
   - 공식 문서: https://code.claude.com/docs/en/plugins
   - 먼저 **플러그인 Scope(적용 범위)** 개념을 설명한다:
     - 설치 시 scope를 묻는 화면이 나옴
     - 아래 테이블로 한눈에 정리해서 보여준다:

     | Scope | 적용 범위 | 설정 저장 경로 | 비고 |
     |-------|----------|--------------|------|
     | **user** | 내 모든 프로젝트 | `~/.claude/settings.json` (Windows: `%USERPROFILE%\.claude\settings.json`) | 나만 사용 |
     | **local** | 이 프로젝트, 나만 | `.claude/settings.local.json` | `.local.json` 컨벤션으로 git commit 대상 아님 |
     | **project** | 이 프로젝트 전체 | `.claude/settings.json` | 팀원과 공유 (git 포함) |

     - (비개발자인 경우에만) local과 project의 차이를 비유로 설명:
       - **local** = "내 책상 서랍에 넣은 도구" — 같은 사무실(프로젝트)이지만 나만 쓸 수 있음. 다른 동료가 이 프로젝트를 열어도 이 플러그인은 안 보임.
       - **project** = "사무실 공용 도구함에 넣은 도구" — 같은 프로젝트를 쓰는 팀원 모두가 함께 사용 가능. git으로 공유됨.
       - **user** = "내가 어디를 가든 항상 들고 다니는 도구" — 어떤 프로젝트를 열든 항상 사용 가능.
     - 실습에서는 **project**를 선택하라고 안내 (my-first-project에서만 쓸 것이므로)
   - Claude Code에서 아래 명령어를 입력하라고 안내:
     ```
     /plugin install skill-creator@claude-plugins-official
     ```
   - 설치 확인: `/plugin` → Installed 탭에서 `skill-creator` 확인
   - 이미 설치되어 있으면 스킵
3. AskUserQuestion으로 준비 완료 여부를 확인한다

---

### 섹션 1: 도구 써보기 (15분)

**목표**: Claude가 쓸 수 있는 도구를 하나씩 직접 체험한다.

> 공식 문서: https://code.claude.com/docs/en/tools

**진행 순서**:

> 실습 시작 전에 "**`my-first-project` 폴더에서 Claude Code를 실행해주세요**"라고 안내한다.
> (OS 감지 결과에 따라 아래 명령어를 안내한다)
> ```bash
> # macOS / Linux
> cd ~/my-first-project
> claude
> ```
> ```powershell
> # Windows (PowerShell)
> cd $HOME\my-first-project
> claude
> ```

먼저 Claude Code가 사용할 수 있는 **기본 도구 전체**를 한눈에 보여준다:

| 도구 | 하는 일 | 비유 |
|------|---------|------|
| **AskUserQuestion** | 사용자에게 질문 | 접수 창구에서 "어떤 용건이신가요?" |
| **Read** | 파일 읽기 | 서류 꺼내 읽기 |
| **Write** | 파일 만들기 | 새 서류 작성 |
| **Edit** | 파일 일부 수정 | 서류에 빨간펜으로 수정 |
| **Bash** | 터미널 명령 실행 | 비서에게 심부름 시키기 |
| **Glob** | 파일 이름으로 검색 | 서류함에서 폴더 찾기 |
| **Grep** | 파일 내용 검색 | 서류 더미에서 키워드 찾기 |
| **WebSearch** | 인터넷 검색 | 구글 검색 대행 |
| **WebFetch** | 웹페이지 읽기 | 특정 URL 내용 가져오기 |
| **Agent** | 하위 작업자 띄우기 | 부하 직원에게 일 맡기기 |

> "이 중에서 오늘은 ⭐ 표시된 4가지를 직접 써볼 겁니다."

그리고 **MCP(Model Context Protocol) 도구**도 소개한다:

> "위의 도구들은 Claude에 **내장된** 네이티브 도구입니다.
> 하지만 Claude는 **MCP 플러그인**을 설치하면 외부 서비스에도 접근할 수 있습니다."

| MCP 플러그인 | 하는 일 | 비유 |
|-------------|---------|------|
| **Atlassian** | Jira 이슈, Confluence 페이지 관리 | 프로젝트 관리 도구 연결 |
| **Google** | Gmail, Calendar, Drive 접근 | 구글 워크스페이스 연결 |
| **Slack** | 슬랙 메시지 읽기/보내기 | 사내 메신저 연결 |
| **Notion** | 노션 페이지 관리 | 문서/위키 연결 |

> "MCP는 Day 3에서 본격적으로 다루지만, 오늘 2개만 미리 설치해봅시다."

**MCP 플러그인 설치 실습**:

1. **Atlassian MCP 설치**: Claude Code에서 아래 명령어를 입력하라고 안내한다:
   ```
   /plugin install atlassian
   ```
   - scope는 **user**를 선택 (모든 프로젝트에서 Jira/Confluence 접근 가능하도록)
   - 설치 과정에서 Atlassian 계정 인증을 진행한다

2. **Google MCP 설치**: Claude Code를 종료한 후, 터미널에서 아래 명령어를 실행하라고 안내한다:
   ```bash
   claude mcp add --scope user --transport http google-mcp https://mcp-servers-int.myrealtrip.net/mcp/google
   ```
   - **주의**: Claude Code 안에서 실행하는 것이 아니라, 터미널에서 직접 실행해야 한다
   - 사내 전용 endpoint(`mcp-servers-int.myrealtrip.net`)를 사용하는 방식이다
   - scope는 `--scope user`로 지정되어 있어 모든 프로젝트에서 사용 가능하다
   - 설치 후 Claude Code를 다시 실행한다

3. **연결 확인**: 설치 후 아래를 입력해보라고 안내한다:
   ```
   내가 접근 가능한 Jira 프로젝트 목록을 보여줘
   ```
   - Claude가 Atlassian MCP 도구를 사용해서 프로젝트 목록을 가져오는 것을 확인
   - "플러그인 하나 설치했을 뿐인데, Claude가 Jira를 읽을 수 있게 되었다"는 점을 강조

4. **핵심 메시지**: "이렇게 MCP 플러그인을 연결하면, Skill에서 '지라에서 이슈 가져와서 정리해줘', '오늘 일정 확인해줘' 같은 지시가 가능해집니다."

AskUserQuestion으로 MCP 플러그인 설치 완료 여부를 확인한다.

이제 네이티브 도구를 하나씩 직접 체험합니다.

#### 도구 1: AskUserQuestion (5분)

1. **도구 설명**:
   - Claude가 사용자에게 **질문**하는 도구
   - **비유**: 접수 창구에서 "어떤 용건이신가요?" 물어보는 것
   - 이게 없으면 Claude가 혼자 독백만 한다
2. **체험 ①: AskUserQuestion 없이** — 사용자에게 아래를 입력하라고 안내한다:
   ```
   나한테 오늘 기분이 어떤지 물어보고, 답변에 맞는 노래를 추천해줘.
   단, AskUserQuestion 도구는 사용하지 마. 텍스트로 질문해.
   ```
   - Claude가 텍스트로 "기분이 어떠세요?"라고 **출력만** 하고 턴이 끝나는 것을 확인하게 한다
   - 사용자는 답변을 **새로운 메시지로 직접 입력**해야 한다
   - "질문은 했지만, 대화가 끊겼다"는 점을 짚어준다
3. **체험 ②: AskUserQuestion 사용** — 이어서 아래를 입력하라고 안내한다:
   ```
   나한테 오늘 기분이 어떤지 AskUserQuestion 도구로 물어보고, 답변에 맞는 노래를 추천해줘
   ```
   - Claude가 AskUserQuestion 도구를 사용하면 **입력 프롬프트**가 뜨는 것을 확인하게 한다
   - 체험 ①과 비교: 텍스트 질문은 일반 출력이라 턴이 끝나지만, AskUserQuestion은 **도구로서 사용자 입력을 받는** 것
4. **핵심 정리**: "AskUserQuestion은 Claude가 사용자에게 질문할 수 있는 **도구**입니다. 텍스트 출력과 달리, 명시적으로 사용자의 입력을 요청하는 도구입니다."

#### 도구 2: Write (3분)

1. **도구 설명**:
   - Write = 파일 만들기, Read = 파일 읽기 (세트로 자주 쓰임)
   - **비유**: 새 서류 작성 / 서류 꺼내 읽기
2. **직접 체험**: 사용자에게 아래를 입력하라고 안내한다:
   ```
   나에 대한 소개를 한 줄로 써서 intro.md 파일로 저장해줘. Write 도구를 사용해.
   ```
   - Claude가 Write 도구로 파일을 생성하는 것을 확인하게 한다
   - "Claude가 직접 파일을 만들었다"는 점을 강조한다

#### 도구 3: Bash (3분)

1. **도구 설명**:
   - 터미널 명령을 실행하는 도구
   - **비유**: 비서에게 심부름 시키기
   - (비개발자인 경우에만) Bash 보충 설명: "Bash는 컴퓨터에게 텍스트로 명령을 내리는 방법입니다. 우리가 마우스로 클릭하는 대신, 글자로 '이거 해줘'라고 입력하는 창이에요. Claude가 이 창을 대신 사용해서 컴퓨터를 조작합니다."
2. **직접 체험**: 사용자에게 아래를 입력하라고 안내한다:
   ```
   Bash 도구를 사용해서 오늘 날짜와 요일을 확인하고, 이번 달 남은 날수를 계산해줘
   ```
   - Claude가 `date` 명령(macOS/Linux) 또는 `Get-Date`(Windows)를 실행하는 것을 확인하게 한다
   - "Claude가 직접 컴퓨터를 조작한다"는 점을 강조한다

#### 도구 4: WebSearch (3분)

1. **도구 설명**:
   - 인터넷 검색 도구
   - **비유**: 구글 검색을 대신 해주는 것
2. **직접 체험**: 사용자에게 아래를 입력하라고 안내한다:
   ```
   WebSearch 도구를 사용해서 최근 제주도 인기 관광지 트렌드를 검색해서 알려줘
   ```
   - Claude가 검색하고 결과를 정리하는 것을 확인하게 한다

#### 도구 5: WebFetch (3분)

1. **도구 설명**:
   - 특정 URL의 웹페이지 내용을 가져오는 도구
   - **비유**: "이 주소에 가서 내용 읽어와" 심부름
   - WebSearch가 "검색"이라면, WebFetch는 "특정 페이지 읽기"
2. **직접 체험**: 사용자에게 아래를 입력하라고 안내한다:
   ```
   WebFetch 도구를 사용해서 https://news.ycombinator.com 에서 지금 인기 있는 글 3개를 요약해줘
   ```
   - Claude가 WebFetch로 페이지 내용을 가져와서 정리하는 것을 확인하게 한다
   - "WebSearch는 뭘 찾을지 모를 때, WebFetch는 어디서 가져올지 알 때" 차이를 짚어준다

#### 중간 정리 (2분)

AskUserQuestion으로 퀴즈를 출제한다:

**Q. 방금 체험한 도구 중에서, Claude가 사용자에게 직접 질문을 던지고 답변을 기다릴 수 있게 해주는 도구는 무엇일까요?**
- A) Read — 파일을 읽는 도구
- B) AskUserQuestion — 사용자에게 질문하고 입력을 받는 도구
- C) Bash — 터미널 명령을 실행하는 도구
- 정답: **B** — AskUserQuestion은 Claude가 사용자에게 명시적으로 질문하고 답변을 기다리는 도구입니다. 단순 텍스트 출력과 달리, 사용자의 입력을 도구로서 요청합니다.

사용자에게 Claude Code를 종료(`/exit`)하라고 안내한다.

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 2: 도구 → Skill (17분)

**목표**: 체험한 도구들을 하나의 Skill로 엮는 법을 배우고, skill-creator를 체험한다.

#### Step 1: Skill 개념 설명 (2분)

1. **왜 Skill이 필요한가**:
   - 방금 4개 도구를 **매번 말로 시키면** 번거로움
   - Skill = 여러 도구를 **순서대로 쓰는 지시사항을 파일로 저장**한 것
   - **비유**: 요리할 때 재료(도구)를 매번 고민하지 않고, **레시피(Skill)를 따라하면 되는 것**

#### Step 2: Skill 파일 구조 설명 (3분)

1. **파일 위치**:
   ```
   프로젝트/
   └── .claude/
       └── skills/                     ← 스킬 폴더
           └── weekly-report/          ← 스킬 이름 = 디렉토리 이름
               ├── SKILL.md            ← 메인 지시사항 (필수)
               └── (참조 파일들)        ← 선택
   ```
   - 디렉토리 이름 = `/명령어` 이름 (`weekly-report/` → `/weekly-report`)
   - SKILL.md = Claude에게 주는 지시사항 (필수)
   - 공식 문서: https://code.claude.com/docs/en/skills

2. **SKILL.md 구조**:
   ```yaml
   ---
   name: 스킬이름                       ← ① `/명령어` 이름
   description: 한 줄 설명              ← ② 이 Skill이 뭔지
   ---

   # 제목                               ← ② 역할 정의

   ## 진행 방식                          ← ③ 단계별 지시사항
   1. AskUserQuestion으로 ~를 물어본다
   2. Bash로 날짜를 확인한다
   3. Write로 파일을 저장한다
   ```

3. **저장 위치에 따른 적용 범위**:
   | 위치 | 경로 (macOS/Linux) | 경로 (Windows) | 적용 범위 |
   |------|-------------------|----------------|----------|
   | 개인용 | `~/.claude/skills/<이름>/SKILL.md` | `%USERPROFILE%\.claude\skills\<이름>\SKILL.md` | 내 모든 프로젝트 |
   | 프로젝트 | `.claude/skills/<이름>/SKILL.md` | `.claude\skills\<이름>\SKILL.md` | 이 프로젝트만 |

AskUserQuestion으로 "여기까지 Skill의 구조가 이해되셨나요? 다음으로 넘어갈까요?"를 확인한다.

4. **Day 1의 CLAUDE.md와 비교**:
   | | CLAUDE.md | SKILL.md |
   |--|-----------|----------|
   | 위치 | 프로젝트 루트 | `.claude/skills/이름/` 안 |
   | 실행 | 항상 자동 | **선택적** 실행 (`/명령어`로 직접 실행하거나, 대화 맥락에서 필요하다고 판단되면 Claude가 자동 호출) |
   | 역할 | "넌 이런 사람이야" | "이 일은 이렇게 해" |

#### Step 3: 직접 만들어보기 — 위키 Q&A Skill (10분)

> 실습생 한 분을 모시고, **Atlassian MCP**를 활용한 실전 Skill을 함께 만들어봅니다.
> Confluence 위키 페이지를 기반으로 질문에 답해주는 Q&A Skill입니다.

**① skill-creator로 만들기**

Claude Code를 실행하고 `/skill-creator`를 입력하라고 안내한다:
(OS 감지 결과에 따라 아래 명령어를 안내한다)
```bash
# macOS / Linux
cd ~/my-first-project
claude
```
```powershell
# Windows (PowerShell)
cd $HOME\my-first-project
claude
```
```
/skill-creator
```

Claude가 인터뷰를 시작하면 아래처럼 답하라고 안내한다:
```
Confluence 위키 페이지를 기반으로 Q&A를 해주는 Skill을 만들고 싶어.

- AskUserQuestion으로 질문을 물어봄
- Atlassian MCP로 관련 Confluence 페이지를 검색
- 검색된 위키 내용을 기반으로 답변을 정리해서 보여줌
- 출처(위키 페이지 제목, 링크)를 함께 표시
- AskUserQuestion으로 추가 질문이 있는지 확인

패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 wiki-qa.
```

- skill-creator가 `.claude/skills/wiki-qa/SKILL.md`를 생성하는 것을 확인

**② 실행해보기**

생성된 Skill을 바로 실행한다:
```
/wiki-qa
```

- 피플팀 관련 질문을 해본다 (예: "연차 신청 절차가 어떻게 되나요?", "복리후생 제도 알려줘")
- Claude가 Confluence에서 검색하고 → 위키 내용 기반으로 답변하는 것을 확인
- 확인 후 Claude Code 종료

**③ 되돌아보기**

핵심 메시지를 전달한다:
- `/skill-creator`에게 원하는 것을 설명했을 뿐인데, MCP를 활용한 Skill이 만들어졌다
- Confluence 위키가 곧 **Q&A 봇의 지식 베이스**가 된 것
- **"Skill + MCP = 외부 서비스를 활용한 자동화"**

AskUserQuestion으로 "위키 Q&A Skill이 잘 동작했나요?"를 확인한다.

#### Step 4: skill-creator로 이메일 브리핑 Skill 만들기 (7분)

> 수동으로 원리를 이해했으니, 이제 **공식 도구**를 사용합니다.
> 앞서 설치한 **Google MCP**를 활용한 실전 Skill을 만들어봅니다.

1. **skill-creator 소개**:
   - Skill을 만들어주는 Skill (메타!)
   - 공식 플러그인: `skill-creator@claude-plugins-official`
   - 대화로 원하는 것을 설명하면, 최적화된 SKILL.md를 자동 생성
   - 더 궁금한 분은 skill-creator의 원본 SKILL.md를 참고: https://github.com/anthropics/claude-plugins-official/blob/main/plugins/skill-creator/skills/skill-creator/SKILL.md

2. **실습: 이메일 브리핑 Skill 만들기**

   Claude Code를 실행하고 `/skill-creator`를 입력하라고 안내한다:
   ```bash
   claude
   ```
   ```
   /skill-creator
   ```

   Claude가 인터뷰를 시작하면 아래처럼 답하라고 안내한다:
   ```
   오늘 받은 이메일을 요약해서 브리핑해주는 Skill을 만들고 싶어.

   - Google MCP의 Gmail 도구로 오늘 받은 이메일 목록을 가져옴
   - 중요도별로 분류 (🔴 긴급/액션 필요, 🟡 참고, 🟢 낮음)
   - 각 이메일의 발신자, 제목, 핵심 내용을 한 줄로 요약
   - 전체 브리핑을 보기 좋게 정리해서 보여줌
   - AskUserQuestion으로 특정 이메일을 더 자세히 볼지 물어봄

   패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 email-briefing.
   ```

   - skill-creator가 `.claude/skills/email-briefing/SKILL.md`를 생성하는 것을 확인
   - 생성 후 바로 `/email-briefing`으로 실행하여 동작 확인
   - **"Google MCP + Skill = 이메일 브리핑 자동화"**
   - 확인 후 Claude Code 종료

AskUserQuestion으로 진행 상황을 확인한다.

#### Step 5: 퀴즈 (2분)

AskUserQuestion으로 아래 퀴즈를 **하나씩** 출제한다:

**Q1. Skill과 CLAUDE.md의 차이는?**
- A) Skill은 선택적 실행, CLAUDE.md는 항상 자동 적용
- B) 둘 다 같은 것
- C) CLAUDE.md가 Skill보다 강력하다
- 정답: **A** — CLAUDE.md는 매 대화 시작 시 자동으로 로딩되어 항상 적용된다. Skill은 `/명령어`로 직접 실행하거나, 대화 맥락에서 필요하다고 판단될 때 선택적으로 작동한다.

**Q2. Skill 파일은 어디에 만들어야 하나요?**
- A) 프로젝트 루트에 `SKILL.md`
- B) `.claude/skills/<이름>/SKILL.md`
- C) `~/.claude/CLAUDE.md`
- 정답: **B** — Skill은 `.claude/skills/` 디렉토리 아래에 스킬 이름으로 된 폴더를 만들고, 그 안에 `SKILL.md` 파일을 넣어야 한다.

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 3: Skill → Skill 오케스트레이션 (15분)

**목표**: Skill끼리 연결하여 더 복잡한 워크플로우를 만드는 법을 배운다.

#### 오케스트레이션 개념 설명 (3분)

> Bash 도구로 아래 OS별 명령어를 실행하여 오케스트레이션 도식 애니메이션을 브라우저에 띄운다. 설명하면서 탭을 전환하며 보여준다.
> - macOS: `open .claude/skills/camp-day2/orchestration-diagram.html`
> - Linux: `xdg-open .claude/skills/camp-day2/orchestration-diagram.html`
> - Windows: `start .claude/skills/camp-day2/orchestration-diagram.html`

1. **오케스트레이션이란?**
   - **비유**: 레고 블록 — 단위 Skill을 조합해서 더 큰 워크플로우를 만드는 것
   - 오늘 만들 오케스트레이션:
     ```
     ┌─────────────────┐   ┌─────────────────┐
     │ /email-briefing │   │ /meeting-recap   │
     │  📧 이메일 요약  │   │  📅 미팅 요약    │
     └────────┬────────┘   └────────┬────────┘
              │                     │
              └──────────┬──────────┘
                         ▼
              ┌─────────────────────┐
              │  /morning-briefing  │
              │  📋 통합 브리핑 출력  │
              └─────────────────────┘
     ```

   - 방법 2가지:
     - Skill을 **따로따로 실행**하되 결과물을 이어받기
     - **하나의 Skill 안에 여러 단계를 묶기** (오케스트레이션)

2. **단위 Skill의 재사용성**:
   - 같은 Skill이 **여러 오케스트레이션에서 재사용**된다:
     ```
     ┌─────────────────┐
     │ /email-briefing │──┬──▶ /morning-briefing  (이메일+미팅 브리핑)
     │  📧 이메일 요약  │  │
     └─────────────────┘  └──▶ /weekly-digest     (이메일+슬랙 주간 요약)

     ┌─────────────────┐
     │ /meeting-recap   │──┬──▶ /morning-briefing  (이메일+미팅 브리핑)
     │  📅 미팅 요약    │  │
     └─────────────────┘  └──▶ /post-meeting      (미팅 후 액션아이템 정리)
     ```
   - 핵심: **한 번 만든 Skill은 여러 곳에서 재활용** — 레고 블록처럼 조합만 바꾸면 새로운 워크플로우 탄생

3. **이런 조합도 가능합니다** — 단위 Skill을 레고처럼 조합:

   **단순 오케스트레이션** (2개 조합):
     ```
     📧 /email-briefing ─┐
                         ├─▶ /morning-briefing     매일 아침 통합 브리핑
     📅 /meeting-recap  ─┘
     ```

   **멀티소스 + 액션** (수집 → 정리 → 전송):
     ```
     📧 /email-briefing ─┐
     📅 /meeting-recap  ─┼─▶ /morning-briefing ──▶ /slack-send
     💬 /slack-collect  ─┘       정리                  전송
     ```

   **파이프라인형** (순차 처리):
     ```
     📅 /meeting-recap ──▶ /action-items ──▶ /jira-create ──▶ /slack-notify
        미팅 요약           액션아이템 추출     Jira 이슈 생성    담당자 알림
     ```

   **팬아웃형** (하나의 결과 → 여러 곳에 배포):
     ```
                             ┌──▶ 💬 슬랙 채널 전송
     📅 /meeting-recap ──▶ ──┼──▶ 🎫 Jira 이슈 생성
                             └──▶ 📝 Confluence 회의록 저장
     ```

#### 실습 3-1: 미팅 내용 수집 Skill 만들기 (5분)

Claude Code를 실행하고 `/skill-creator`로 미팅 수집 Skill을 만들라고 안내한다:

```bash
claude
```
```
/skill-creator
```

```
어제 있었던 미팅 내용을 수집해서 정리해주는 Skill을 만들어줘.

- Google MCP의 Calendar 도구로 어제 하루의 미팅(일정) 목록을 가져옴
- 각 미팅의 제목, 시간, 참석자를 정리
- 미팅 설명(description)이나 첨부된 메모가 있으면 핵심 내용을 요약
- 결과를 보기 좋게 정리해서 보여줌

패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 meeting-recap.
```

- `.claude/skills/meeting-recap/SKILL.md`가 생성되는 것을 확인
- 확인 후 Claude Code 종료

AskUserQuestion으로 미팅 수집 Skill 생성 완료 여부를 확인한다.

#### 실습 3-2: 오케스트레이션 체험 — 이메일 + 미팅 따로 실행 (3분)

Claude Code를 실행하라고 안내한다:

```bash
claude
```

먼저 이메일 브리핑을 실행:
```
/email-briefing
```

이어서 미팅 수집을 실행:
```
/meeting-recap
```

- 두 Skill을 각각 실행해서 결과를 확인하게 한다
- "각각은 잘 동작하지만, 따로따로 실행하니 번거롭다"는 점을 짚어준다
- "이 두 개를 하나로 묶으면 어떨까?" 질문으로 다음 실습으로 유도한다

#### 실습 3-3: 풀 프로세스 Skill — 모닝 브리핑 (4분)

두 단계를 하나로 묶는 오케스트레이션 Skill을 skill-creator로 만든다:

```
/skill-creator
```

```
이메일과 미팅 내용을 한번에 수집해서 통합 브리핑을 만들어주는 Skill을 만들어줘.

Step 1 - 이메일 수집:
- Google MCP의 Gmail 도구로 오늘 받은 이메일을 가져옴
- 중요도별로 분류 (🔴 긴급/액션 필요, 🟡 참고, 🟢 낮음)
- 각 이메일의 발신자, 제목, 핵심 내용을 한 줄로 요약

Step 2 - 미팅 수집:
- Google MCP의 Calendar 도구로 어제 미팅 목록을 가져옴
- 각 미팅의 제목, 시간, 참석자, 핵심 내용을 정리

Step 3 - 통합 브리핑 출력:
- 이메일 요약과 미팅 요약을 하나의 보기 좋은 브리핑으로 합침
- "📧 이메일 브리핑" 섹션과 "📅 미팅 리캡" 섹션으로 구분
- 오늘 주의해야 할 포인트를 마지막에 정리

패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 morning-briefing.
```

실행:
```
/morning-briefing
```

- 하나의 명령어로 **이메일 수집 → 미팅 수집 → 통합 브리핑**이 자동으로 흘러가는 것을 체험하게 한다
- **"이것이 오케스트레이션의 힘 — 여러 소스를 한 번에 모아서 정리"**
- 확인 후 Claude Code 종료

#### 퀴즈 (2분)

AskUserQuestion으로 퀴즈를 출제한다:

**Q. 오케스트레이션의 장점은?**
- A) Skill 파일이 줄어든다
- B) 여러 단계의 작업을 한 명령어로 자동 실행할 수 있다
- C) Claude가 더 똑똑해진다
- 정답: **B** — 오케스트레이션은 여러 Skill의 단계를 연결하여, 하나의 `/명령어`로 전체 워크플로우를 자동 실행하는 것이다.

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 4: 플러그인과 마켓플레이스 (10분)

**목표**: 플러그인이 무엇인지 이해하고, 마켓플레이스에서 쇼핑해본다.

#### 플러그인이란? (3분)

1. **플러그인 = 앱**:
   - 우리가 만든 Skill은 **하나의 기능** (예: `/email-briefing`)
   - **플러그인**은 여러 Skill, 커맨드, 에이전트 등을 **하나로 묶어서 배포**하는 패키지
   - **비유**: 스마트폰의 **앱** — 하나의 앱 안에 여러 기능이 들어있는 것처럼, 플러그인도 여러 Skill을 묶어놓은 것

2. **마켓플레이스 = 앱스토어**:
   - 만든 플러그인을 **마켓플레이스에 올리면** 다른 사람들도 설치해서 사용 가능
   - 비유 정리:

     | 스마트폰 | Claude Code |
     |---------|------------|
     | 앱스토어 | 마켓플레이스 (`/plugin` → Available) |
     | 앱 | 플러그인 |
     | 앱의 각 기능 | Skill, 커맨드, 에이전트 |
     | 앱 설치 | `/plugin install ...` |

   - **서드파티 마켓플레이스도 가능**:
     - 공식 마켓플레이스 외에, **회사 전용 마켓플레이스**를 직접 만들 수 있음
     - 비유: SKT가 원스토어를 만든 것처럼, 우리 회사만의 전용 앱스토어를 운영하는 것
     - 실제로 **우리 회사에도 자체 마켓플레이스**가 있음 — 사내 Skill/플러그인을 여기서 공유
     - 회사 보안 정책에 맞는 플러그인만 승인/배포 가능

3. **Skill 배포 단계 정리**:

   | 단계 | 방법 | 비유 |
   |------|------|------|
   | 나만 쓰기 | `~/.claude/skills/` 또는 `.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`) | 내 레시피를 수첩에 적어두기 |
   | 팀과 공유 | `.claude/skills/` + git push | 팀 공유 폴더에 레시피 올리기 |
   | 플러그인으로 배포 | 마켓플레이스에 등록 | 요리책 출판해서 앱스토어에 올리기 |

#### 플러그인 쇼핑 — superpowers 체험 (7분)

> "앱스토어를 둘러보듯, 마켓플레이스에서 플러그인을 쇼핑해봅시다!"

1. **마켓플레이스 둘러보기**: Claude Code에서 아래 명령어를 입력하라고 안내한다:
   ```
   /plugin
   ```
   - **Available** 탭에서 설치 가능한 플러그인 목록을 둘러보게 한다
   - "앱스토어에서 앱 목록을 구경하는 것과 같다"고 설명한다

2. **superpowers 플러그인 설치**: 아래 명령어를 입력하라고 안내한다:
   ```
   /plugin install superpowers
   ```
   - scope는 **project**를 선택

3. **플러그인 안의 Skill 구경하기**:
   - 설치 후 `/` 을 입력해서 새로 추가된 명령어들을 확인하게 한다
   - "하나의 플러그인을 설치했을 뿐인데, 여러 개의 `/명령어`가 한꺼번에 추가되었다"는 점을 강조
   - 마음에 드는 명령어를 하나 골라서 실행해보게 한다
   - **"이것이 플러그인의 힘 — 누군가가 만든 여러 Skill을 한 번에 설치"**

4. **핵심 메시지**:
   - "오늘 우리가 만든 `/email-briefing`, `/meeting-recap`, `/morning-briefing`도 플러그인으로 묶으면 다른 사람과 공유할 수 있습니다"
   - "좋은 Skill은 팀 전체의 생산성을 높입니다 — 한 사람이 만들면 모두가 씁니다"
   - 공식 문서: https://code.claude.com/docs/en/plugins

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 5: Wrap-up - 오늘 배운 것 (5분)

**목표**: 학습 내용을 정리하고 종합 퀴즈로 확인한다.

**진행 순서**:

1. 오늘 배운 3단계를 정리한다:

   | 단계 | 비유 | 배운 것 |
   |------|------|---------|
   | 🧱 도구 | 망치, 톱, 자 | AskUserQuestion, Read/Write, Bash, WebSearch |
   | 🏗️ Skill | 조립 설명서 | `.claude/skills/<이름>/SKILL.md`에 도구들을 엮기 |
   | 🏙️ 오케스트레이션 | 가구 세트 | Skill의 출력 → 다음 Skill의 입력으로 연결 |

2. 3일간의 여정을 보여준다:
   ```
   Day 1: Claude의 성격을 설정했다        (CLAUDE.md, Memory)
   Day 2: Claude에게 기술을 가르쳤다       (도구 → Skill → 오케스트레이션) ← 오늘
   Day 3: Claude의 능력을 확장한다         (MCP — 외부 도구 연결)
   ```

3. AskUserQuestion으로 종합 퀴즈를 **하나씩** 출제한다:

**Q1. 도구, Skill, 오케스트레이션의 관계를 올바르게 설명한 것은?**
- A) 도구는 Skill 안에서만 쓸 수 있다
- B) 도구를 조합하면 Skill, Skill을 연결하면 오케스트레이션
- C) 오케스트레이션은 코딩을 해야만 가능하다
- 정답: **B** — 도구(Tool)는 하나의 동작, Skill은 여러 도구를 엮은 워크플로우, 오케스트레이션은 Skill끼리 연결한 자동화 파이프라인이다.

**Q2. Skill을 만드는 가장 효율적인 방법은?**
- A) SKILL.md를 처음부터 수동으로 작성
- B) `/skill-creator`로 대화하며 생성
- C) 다른 사람의 Skill을 복사
- 정답: **B** — `/skill-creator`는 공식 플러그인으로, 대화로 원하는 것을 설명하면 최적화된 SKILL.md를 자동 생성해준다.

**Q3. 다음 중 Skill의 `description` 필드의 역할은?**
- A) 사용자에게 보여주는 도움말
- B) Claude가 언제 이 Skill을 쓸지 판단하는 근거
- C) Skill의 실행 순서를 결정
- 정답: **B** — `description`은 Claude가 사용자의 요청에 맞는 Skill을 자동으로 선택할 때 참고하는 설명이다.

4. 다음 단계를 안내한다:
   - 만든 Skill을 실제 업무에서 한 번 써보기
   - 불편한 점이 있으면 `/skill-creator`로 Skill 수정해보기
   - "이것도 자동화할 수 있을까?" 목록 적어오기 (Day 3 준비)

5. Day 3 예고:
   > "오늘 만든 Skill이 슬랙, 이메일, 노션까지 접근할 수 있다면? Day 3에서 MCP로 확장합니다."

---

## 진행 규칙

- **퀴즈 진행**: `use camp-quiz` 패턴을 따르세요 — AskUserQuestion으로 선택지 출제, 사용자 답변 후 정답 공개, 맞으면 칭찬/틀리면 해설.
- **섹션 전환**: `use camp-section-transition` 패턴을 따르세요 — 섹션 핵심 요약 후 남은 시간과 함께 다음 섹션 진행 여부를 확인. 각 섹션별 누적 시간 기준:
  - 섹션 0 끝 (3분 경과) → "남은 시간: 약 65분"
  - 섹션 0.5 끝 (6분 경과) → "남은 시간: 약 62분"
  - 섹션 1 끝 (21분 경과) → "남은 시간: 약 47분"
  - 섹션 2 끝 (38분 경과) → "남은 시간: 약 30분"
  - 섹션 3 끝 (53분 경과) → "남은 시간: 약 15분"
  - 섹션 4 끝 (63분 경과) → "남은 시간: 약 5분"
- 실습 단계에서는 사용자가 **별도의 터미널**에서 직접 명령어를 실행하도록 안내하되, 막히면 도와주세요.
- 사용자가 질문하면 교안 내용을 기반으로 답변하세요.
- 교안에 없는 내용이면 솔직히 말하고, 관련 레퍼런스를 안내하세요.
- 설명할 때는 비유를 적극 활용하세요. 교안에 있는 비유를 우선 사용하세요.
- `/skill-creator` 실습에서는 "패키징 하지 말고, 현재 디렉토리에 바로 만들어줘"를 안내하세요.
