# Day 4: 스킬 배포와 확장 — 플러그인에서 Agent SDK까지

> **부제**: 만든 것을 퍼뜨리고, AI 에이전트가 쓸 수 있는 도구를 만들어라

---

## 메타 정보

| 항목 | 내용 |
|------|------|
| **시간** | 90분 (버퍼 30분 포함, 총 120분 슬롯) |
| **난이도** | 입문~중급 |
| **선수 과목** | Day 1 (CLAUDE.md + Memory), Day 2 (Skill 만들기), Day 3 (Agent + 병렬 작업) |
| **대상** | 개발자 + 비개발자 (마케팅, 세일즈, 전략, 재무, 운영) |

---

## 핵심 메시지

> **"AI 에이전트가 쓸 수 있는 도구를 만들어라."**
>
> 소프트웨어를 만들 때도, 그 소프트웨어가 MCP 혹은 Skill 형태로 AI 에이전트가 사용할 수 있는 구조여야 확장성을 확보할 수 있다.
> 만약 딱 하나만 만들 수 있다면 — GUI가 아니라 **Skill**을 만들어라.

---

## 4일간의 여정 — 전체 맥락

```
Day 1: 나를 위한 세팅       (CLAUDE.md + Memory)
Day 2: 나만의 워크플로우     (Skill 만들기)
Day 3: 나만의 AI 팀         (Agent + 병렬 작업)
Day 4: 만든 것을 퍼뜨리기   (Plugin 배포 + Agent SDK) ← 오늘
```

Day 1~3에서 수강생은 자신만의 CLAUDE.md를 만들고, Skill을 만들고, Agent를 조합했다.
Day 4는 이 결과물을 **다른 사람이 쓸 수 있게** 만드는 방법을 다룬다.

---

## 섹션 구성 요약

| 섹션 | 주제 | 시간 | 형태 |
|------|------|------|------|
| 0 | Recap + 오늘의 여정 | 5분 | 강의 |
| 1 | 마켓플레이스 체험 — 설치해보기 | 20분 | **실습** |
| 2 | 플러그인 만들기 — 내 스킬을 패키징 | 25분 | **실습** |
| 3 | 무엇을 만들 것인가 — 관점 전환 | 10분 | 대화형 워크숍 |
| 4 | Agent SDK 데모 — mari로 보는 확장 | 15분 | 데모 + **참여** |
| 5 | Wrap-up — 4일의 여정을 마치며 | 15분 | 회고 + 졸업식 |
| | **합계** | **90분** | 실습 ~50% |

---

## 섹션별 상세

---

### 섹션 0: Recap + 오늘의 여정 (5분)

**목표**: Day 1~3 복습, Day 4의 질문 던지기

#### 4일 여정 다이어그램

```
+------------------+     +------------------+     +------------------+     +------------------+
|     Day 1        |     |     Day 2        |     |     Day 3        |     |     Day 4        |
|                  |     |                  |     |                  |     |                  |
| "Setting Up"     | --> | "Skill Up"       | --> | "Team Up"        | --> | "Scale Up"       |
|                  |     |                  |     |                  |     |                  |
| CLAUDE.md        |     | Skill            |     | Agent            |     | Plugin           |
| Memory           |     | Skill Chaining   |     | Parallel Work    |     | Marketplace      |
|                  |     |                  |     |                  |     | Agent SDK        |
+------------------+     +------------------+     +------------------+     +------------------+
     for ME                  for MY WORK            for MY TEAM             for EVERYONE
```

#### 진행

- Day 1~3에서 만든 것 되짚기
  - CLAUDE.md → 나의 매뉴얼
  - Skill → 나의 워크플로우
  - Agent → 나의 AI 팀
- **오늘의 질문**: "이걸 옆 자리 동료도 쓸 수 있게 하려면?"
- 오늘 배울 것:
  1. 마켓플레이스에서 플러그인 설치하기 (소비자 입장)
  2. 내 스킬을 플러그인으로 패키징하기 (생산자 입장)
  3. 스킬을 Slack 같은 다른 채널로 확장하기 (Agent SDK)

#### 오늘의 인지 프레임: "메타 지식의 3단계"

오늘 강의에서 여러분이 **알아야 할 것**과 **몰라도 되는 것**을 먼저 정리하겠습니다.

Claude Code를 도구로 사용할 때, 우리에게 필요한 건 **코드를 직접 쓰는 능력**이 아닙니다.
**내가 쓰는 도구가 어떻게 동작하는지** — 이것만 알면 됩니다.

문제가 생겼을 때의 사고 흐름:

```
1. Detect     "안 된다" -- 기대와 현상의 불일치를 감지
      |
2. Suspect    "어디가 문제일까" -- 의심 대상 목록을 안다
      |
3. Verify     "어떻게 확인하지" -- 검증 경로를 안다
```

**알아야 할 것 vs 몰라도 되는 것**:

| 알아야 할 것 (Do) | 몰라도 되는 것 (Don't) |
|---|---|
| 플러그인 디렉토리 구조 (`.claude-plugin/`, `skills/`, `agents/`) | plugin.json의 JSON 문법을 직접 작성하는 것 |
| plugin.json이 어디에 위치하고 무슨 역할인지 | plugin.json의 모든 필드 스펙을 외우는 것 |
| `--plugin-dir`로 로컬 테스트하는 방법 | 마켓플레이스 서버 인프라 구조 |
| 마켓플레이스와 플러그인의 1:N 관계 | marketplace.json의 세부 필드 |
| "안 될 때 어디를 봐야 하는지" (의심 대상 목록) | 에러 로그를 직접 읽고 디버깅하는 것 |
| Claude에게 **공식 문서 기반**으로 질문하는 법 | Claude Code 내부 동작 원리 |
| `claude-code-guide` 에이전트 멘션 방법 | Agent SDK 코드를 직접 작성하는 것 |
| Skill → Plugin → Marketplace 흐름 전체 그림 | Git/GitHub PR 프로세스의 기술적 세부사항 |

> **핵심**: 작성은 Claude가, 검증은 내가. 단, 검증하려면 **구조는 알아야** 합니다.

---

### 섹션 1: 마켓플레이스 체험 — 설치해보기 (20분)

**목표**: 플러그인 생태계를 **소비자** 입장에서 먼저 체험

> Day 2에서 스킬을 배울 때도 "먼저 써보고, 그다음 만들기" 순서였습니다.
> 오늘도 같은 패턴 — 먼저 설치해보고, 그다음 만들어봅시다.

#### 1-1. 플러그인이란? (3분)

**개념 설명**

- **비유**: 스킬이 "레시피"라면, 플러그인은 **"레시피북"** — 여러 레시피를 묶어서 한 번에 전달
- 플러그인 = 스킬 + 에이전트 + MCP 서버를 하나의 패키지로 묶은 것
- 마켓플레이스 = 레시피북을 모아놓은 **"서점"** — 검색하고 골라서 설치

```
+-------------------+
|   Marketplace     |    <-- 서점 (여러 플러그인을 모아놓은 곳)
|  +-----------+    |
|  | Plugin A  |    |    <-- 레시피북 (스킬+에이전트 묶음)
|  | - Skill 1 |    |    <-- 레시피
|  | - Skill 2 |    |
|  | - Agent 1 |    |
|  +-----------+    |
|  +-----------+    |
|  | Plugin B  |    |
|  | - Skill 3 |    |
|  +-----------+    |
+-------------------+
```

- **마켓플레이스와 플러그인의 관계 (1:N)**
  - 하나의 마켓플레이스에 여러 플러그인이 등록됨
  - `marketplace.json`은 마켓플레이스 루트에 하나
  - 각 플러그인 안에 `plugin.json`이 각각 존재

#### 1-2. 전사 마켓플레이스 소개 (2분)

- **우리 회사의 마켓플레이스**: https://github.com/myrealtrip/claude-code-plugins
  - 현재 15개 플러그인 등록
  - `mrt-devkit`, `techspec-builder`, `strategy-builder`, `workflow-utils` 등
- 실제 구조 보여주기 (GitHub 화면 또는 터미널)

```
myrealtrip/claude-code-plugins/        ← 마켓플레이스 (서점)
├── .claude-plugin/
│   └── marketplace.json               ← 전체 플러그인 목록
├── mrt-devkit/                        ← 플러그인 1 (레시피북)
│   ├── .claude-plugin/
│   │   └── plugin.json                ← 이 플러그인의 메타데이터
│   ├── skills/
│   │   ├── bootstrap/SKILL.md
│   │   ├── setup/SKILL.md
│   │   └── visualize/SKILL.md
│   └── agents/
│       └── dev-guide.md
├── techspec-builder/                  ← 플러그인 2
│   └── ...
├── strategy-builder/                  ← 플러그인 3
│   └── ...
└── ... (15개 플러그인)
```

#### 1-3. 실습: 마켓플레이스 추가 + 플러그인 설치 (15분)

**실습 A: 전사 마켓플레이스(mrt-devkit) 설치**

Claude Code 안에서 `/plugin` 명령어 실행:

```
Step 1: 마켓플레이스 추가
> claude plugin marketplace add myrealtrip/claude-code-plugins

Step 2: 플러그인 설치
> claude plugin install mrt-devkit

Step 3: 확인
> claude plugin list
```

설치 후 확인:
- `/mrt-devkit:bootstrap` 같은 새로운 슬래시 커맨드가 생겼는지 확인
- "아, 누군가 만든 스킬을 이렇게 쉽게 가져다 쓸 수 있구나!"

**실습 B: oh-my-claudecode 설치**

```
Step 1: 마켓플레이스 추가
> claude plugin marketplace add Yeachan-Heo/oh-my-claudecode

Step 2: 플러그인 설치
> claude plugin install oh-my-claudecode

Step 3: 확인
> claude plugin list
```

설치 후 확인:
- `/oh-my-claudecode:autopilot`, `/oh-my-claudecode:team` 같은 고급 스킬들
- 28개 에이전트, 32개+ 스킬이 한 번에 설치됨

**핵심 체감 포인트**:
> "플러그인 2줄이면 다른 사람이 만든 수십 개의 스킬과 에이전트를 바로 쓸 수 있다."

#### Tip: 설치가 안 될 때

> **"안 된다"를 감지하는 법**:
> - `claude plugin list`를 실행했는데 방금 설치한 플러그인이 목록에 없다
> - `/mrt-devkit:bootstrap`을 입력했는데 "unknown command"가 나온다
>
> **의심 대상**:
> - 마켓플레이스가 제대로 추가됐나? → `claude plugin marketplace list`로 확인
> - 플러그인 이름을 정확히 입력했나? → `claude plugin list --available --json`으로 확인
>
> **검증 경로**:
> - Claude Code에서 `@claude-code-guide`를 멘션하며 질문:
>   *"@claude-code-guide 플러그인을 설치했는데 슬래시 커맨드가 보이지 않아요. 어떻게 확인하나요?"*
> - 또는 공식 문서 URL을 함께 제공하며 질문:
>   *"https://docs.claude.ai/en/docs/plugins 이 문서를 참고해서 답변해줘"*
>
> **왜 이렇게 질문해야 하나요?**
> Claude에게 그냥 물어보면 잘못된 정보를 줄 수 있습니다.
> **공식 문서 기반**으로 답하도록 유도해야 정확한 답을 얻습니다.

#### 퀴즈 (use camp-quiz)

> **Q. 마켓플레이스와 플러그인의 관계로 올바른 것은?**
>
> A) 마켓플레이스 1개 = 플러그인 1개 (1:1)
> B) 마켓플레이스 1개에 여러 플러그인 등록 가능 (1:N)
> C) 플러그인 1개에 여러 마켓플레이스 등록 (1:N)
>
> **정답**: B — 마켓플레이스는 "서점", 플러그인은 "책". 한 서점에 여러 책이 있다.

---

### 섹션 2: 플러그인 만들기 — 내 스킬을 패키징 (25분)

**목표**: 내가 만든 스킬을 플러그인으로 패키징하고 로컬에서 테스트

> 이제 소비자에서 **생산자**로 전환합니다.
> Day 2에서 만든 스킬을 플러그인으로 묶어봅시다.

#### 2-1. 플러그인 구조 이해 (5분)

전사 마켓플레이스의 `mrt-devkit`을 실제 예시로 살펴보기:

**plugin.json** (mrt-devkit 실제 파일):
```json
{
  "name": "mrt-devkit",
  "description": "MyRealTrip development harness...",
  "version": "0.4.0",
  "author": { "name": "Donghoon Lee" },
  "license": "MIT",
  "skills": [
    "./skills/bootstrap/SKILL.md",
    "./skills/setup/SKILL.md",
    "./skills/visualize/SKILL.md"
  ],
  "agents": [
    "./agents/dev-guide.md"
  ]
}
```

핵심 필드:
- `name`: 플러그인 이름 (설치 시 사용)
- `description`: 설명
- `version`: 버전
- `skills`: 스킬 파일 경로 목록
- `agents`: 에이전트 파일 경로 목록

#### 2-2. 실습: 내 플러그인 만들기 (15분)

**Claude에게 시키기 패턴** (Day 2의 skill-creator와 동일한 접근):

> 비개발자도 따라올 수 있도록, JSON을 직접 작성하지 않습니다.
> Claude에게 자연어로 지시하면 Claude가 구조를 만들어줍니다.

**Step 1: 플러그인 폴더 구조 생성**

Claude Code에서 다음과 같이 지시:

```
"Day 2에서 만든 [스킬 이름] 스킬을 플러그인으로 패키징해줘.
플러그인 이름은 'my-first-plugin'으로 하고,
~/my-first-plugin/ 디렉토리에 만들어줘."
```

Claude가 생성하는 결과:

```
~/my-first-plugin/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── my-skill/
        └── SKILL.md        ← Day 2에서 만든 스킬
```

**Step 2: 로컬에서 테스트**

`--plugin-dir` 플래그로 플러그인을 로컬 테스트:

```bash
# 플러그인 디렉토리를 지정해서 Claude Code 실행
claude --plugin-dir ~/my-first-plugin
```

Claude Code가 열리면:
- `/my-skill` 슬래시 커맨드가 보이는지 확인
- 실행해서 정상 동작하는지 테스트

**Step 3: 검증**

```bash
# 플러그인 구조가 올바른지 검증
claude plugin validate ~/my-first-plugin
```

#### 2-3. 마켓플레이스에 올리려면? (5분)

> 로컬 테스트가 끝났으면, 이제 팀에 공유할 차례입니다.

**전사 마켓플레이스에 등록하는 흐름** (개념 설명):

```
[내 플러그인 로컬 테스트 완료]
        |
        v
[GitHub에 Push]
   - 방법 1: 전사 마켓플레이스 repo에 PR (myrealtrip/claude-code-plugins)
   - 방법 2: 자기 repo를 새 마켓플레이스로 등록
        |
        v
[동료가 설치]
   > claude plugin marketplace add <repo>
   > claude plugin install <plugin-name>
```

**전사 마켓플레이스에 PR 보내기** (실제 흐름):
1. `myrealtrip/claude-code-plugins` 레포를 fork
2. 내 플러그인 디렉토리 추가
3. `marketplace.json`의 plugins 배열에 내 플러그인 등록
4. PR 생성 → 리뷰 → 머지

> 이 과정은 오늘 실습하지 않습니다. 핵심은 **로컬에서 플러그인이 동작하는 것을 확인**한 것입니다.
> 마켓플레이스 등록은 강의 후 각자 해보세요!

#### Tip: 플러그인이 동작하지 않을 때

> **트러블슈팅 사고 흐름**:
>
> ```
> 스킬이 .claude/skills/ 에서는 잘 됐는데
>       |
> 플러그인으로 감싼 후 --plugin-dir 로 실행하니 안 된다
>       |
> 그러면 문제는 "플러그인 패키징 과정"에 있다
>       |
> 의심 대상:
>   1. .claude-plugin/plugin.json 이 존재하는가?
>   2. plugin.json 의 skills 경로가 실제 파일 위치와 맞는가?
>   3. SKILL.md 파일이 정확한 위치에 있는가?
> ```
>
> **검증 방법**:
> ```bash
> # 1단계: 구조 검증 (기계에게 맡기기)
> claude plugin validate ~/my-first-plugin
>
> # 2단계: 직접 확인 (내가 알아야 할 것)
> ls ~/my-first-plugin/.claude-plugin/plugin.json  # 파일 존재?
> ls ~/my-first-plugin/skills/my-skill/SKILL.md     # 스킬 파일 존재?
> ```
>
> **그래도 안 되면? — `claude-code-guide` 에이전트 활용**:
>
> Claude Code 안에서 이렇게 질문하세요:
> ```
> @claude-code-guide 플러그인을 만들었는데 --plugin-dir로 로드가 안 됩니다.
> plugin.json의 올바른 구조를 알려주세요.
> ```
>
> `claude-code-guide`는 Claude Code 공식 문서를 기반으로 답변하는 전문 에이전트입니다.
> 일반 질문보다 **훨씬 정확한 답**을 얻을 수 있습니다.

#### Tip: 알아야 할 것의 기준

> **"이 도구를 최적으로 쓰고 싶다면, 도구의 구조는 알아야 한다."**
>
> 코드를 직접 쓰지 않아도 됩니다. Claude가 대신 써줍니다.
> 하지만 Claude가 만든 결과가 맞는지 틀린지는 **내가** 판단해야 합니다.
>
> 그 판단을 하려면:
> - **디렉토리 구조**를 알아야 합니다 (어떤 파일이 어디에 있어야 하는지)
> - **핵심 파일의 역할**을 알아야 합니다 (plugin.json이 뭘 하는 파일인지)
> - **문제가 생겼을 때 어디를 봐야 하는지** 알아야 합니다
>
> 이것이 "메타 지식" — 코드를 쓰는 능력이 아니라, **도구를 이해하는 능력**입니다.

#### 퀴즈 (use camp-quiz)

> **Q. 로컬에서 만든 플러그인을 테스트하려면?**
>
> A) `claude plugin install ~/my-plugin`
> B) `claude --plugin-dir ~/my-plugin`
> C) `claude plugin test ~/my-plugin`
>
> **정답**: B — `--plugin-dir` 플래그로 세션에서만 로드하여 테스트할 수 있다.

---

### 섹션 3: 무엇을 만들 것인가 — 관점 전환 (10분)

**목표**: "GUI vs Skill" 관점 전환을 대화형으로 전달

> 섹션 1~2에서 플러그인을 만들고 설치해봤습니다.
> 이제 한 발짝 물러서서, **어떤 형태로 만들 것인가**를 생각해봅시다.

#### 3-1. 투표 (AskUserQuestion)

> **여러분이 다음에 만들고 싶은 건 무엇인가요?**
>
> A) 웹 대시보드 / 관리자 페이지
> B) Slack bot / 챗봇
> C) Skill / MCP 도구
> D) 잘 모르겠다

(투표 결과를 받은 후 대화 진행)

#### 3-2. 핵심 프레임 제시

우리는 지금 AI 에이전트를 통해 일하고 있습니다.
- 우리의 손과 뇌는 **Claude Code와 소통**하고 있습니다
- 모든 일은 **Claude가 대신** 해주고 있습니다

그렇다면 Claude에게 최적화된 입력을 줄 수 있어야 합니다.

**핵심 질문**: "당신이 만든 시스템은 **누가** 쓸 건가요?"

```
                    What I Build
                         |
              +----------+----------+
              |                     |
      [Human uses it]        [AI Agent uses it]
              |                     |
     GUI / Web / Slack Bot    Skill / Agent / MCP
              |                     |
    "Expansion stops here"  "Expansion continues"
```

#### 3-3. 분기점 — 실전 적용

| 질문 | → GUI/Web/Bot | → Skill/Agent/MCP |
|------|:---:|:---:|
| 이 시스템을 더 이상 AI가 확장할 필요 없는가? | ✅ | |
| 사람만 쓸 최종 제품인가? | ✅ | |
| 다른 AI 에이전트가 이 시스템을 호출해야 하는가? | | ✅ |
| 에이전트 → 에이전트 체이닝으로 확장하고 싶은가? | | ✅ |

**수강생 업무에 대입하기** (AskUserQuestion):

> **여러분이 자동화하고 싶은 업무 하나를 떠올려보세요.**
> 그 업무를 위 표에 대입하면, 어느 쪽에 가까운가요?
>
> A) 사람이 최종 소비자 → GUI/Bot 쪽
> B) AI 에이전트가 활용 → Skill/MCP 쪽
> C) 둘 다 필요할 것 같다

#### 3-4. 메시지

> 웹서비스, 슬랙봇을 **꼭 만들어야 한다는 강박에 빠지지 마세요.**
>
> 소프트웨어를 만들더라도, MCP 혹은 Skill 형태로 AI 에이전트가 사용할 수 있는 구조가 되어야 확장성을 확보할 수 있습니다.
>
> **만약 딱 하나만 만들 수 있다면 — GUI가 아니라 Skill을 만드세요.**
> Skill을 먼저 만들면, 그 위에 GUI든 Slack bot이든 얹을 수 있습니다.
> 하지만 GUI를 먼저 만들면, AI 에이전트가 쓰기 어렵습니다.

---

### 섹션 4: Agent SDK 데모 — mari로 보는 확장 (15분)

**목표**: 스킬이 Claude Code 밖으로 나갈 수 있다는 가능성을 체험

> 섹션 3에서 "Skill을 먼저 만들면 그 위에 뭐든 얹을 수 있다"고 했죠?
> 그 실제 사례를 보여드리겠습니다.

#### 4-1. Agent SDK 소개 (3분)

- **Agent SDK란?**: Claude Code의 핵심 엔진을 **프로그래밍 가능한 SDK**로 꺼낸 것
- Agent SDK는 내부적으로 Claude Code에 기반 → Claude Code에서 쓰던 **Skill, Agent, MCP**를 그대로 사용 가능
- 즉, Claude Code에서 채팅하며 쓰던 스킬을 **복사 붙여넣기로** 다른 환경에 옮길 수 있음

```
+------------------+        +------------------+
|  Claude Code     |        |  Agent SDK App   |
|  (Terminal)      |        |  (Slack, Web...) |
|                  |        |                  |
|  +------------+  |  copy  |  +------------+  |
|  | Skill      |--|------->|  | Skill      |  |
|  | Agent      |  |  paste |  | Agent      |  |
|  | MCP        |  |        |  | MCP        |  |
|  +------------+  |        |  +------------+  |
+------------------+        +------------------+
      Same Skills, Different Surface
```

#### 4-2. mari 소개 + 데모 (5분)

**mari**: 임수진님이 Agent SDK로 만든 Slack bot

- **핵심**: Skill을 그대로 가져다 Slack chatbot화
- Skill이 핵심이고, Slack은 하나의 **출구(surface)** 일 뿐
- 이것이 섹션 3에서 말한 "Skill을 먼저 만들면 그 위에 뭐든 얹을 수 있다"의 실제 사례

**데모**: `01_협업_ai_lab` 채널에서 실제 시연
- Slack에서 mari에게 메시지 → 스킬이 실행되는 모습
- Claude Code에서의 경험과 동일한 사용자 경험

#### 4-3. 참여형 체험 (7분)

**전체 수강생 참여: mari에게 메시지 보내기**

> 모두 Slack `01_협업_ai_lab` 채널에 접속해주세요.
> mari에게 직접 메시지를 보내봅시다.

- 각자 mari에게 간단한 질문이나 요청을 보내보기
- "Claude Code에서 하던 것과 똑같은 경험이 Slack에서도 된다"를 체감

**스킬 배포 시연 (1명 지원자)**

자신이 만든 스킬이 있는 수강생 중 지원자 1명:
- 자신의 스킬을 mari에 배포하는 과정을 화면 공유로 시연
- 배포 후 다른 수강생들이 Slack에서 그 스킬을 호출해보기

> **참여 포인트**: 시연자 1명이 배포하지만, **전체 수강생이 Slack에서 결과를 테스트**합니다.
> "내가 만든 스킬이 Slack에서 돌아가는 걸 동료들이 바로 쓰는" 순간을 체험합니다.

#### Agent SDK 코드 (참고용)

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

// Claude Code에서 하던 것과 동일한 에이전트 루프
for await (const message of query({
  prompt: "사용자의 요청",
  options: {
    allowedTools: ["Read", "Edit", "Bash", "Skill"],
    permissionMode: "acceptEdits"
  }
})) {
  // Slack으로 스트리밍 출력
  sendToSlack(message);
}
```

> **참고**: Agent SDK 자체를 직접 다루는 건 소프트웨어 개발 영역입니다.
> 오늘의 핵심은 "Skill을 잘 만들면 어디든 옮길 수 있다"는 것입니다.

---

### 섹션 5: Wrap-up — 4일의 여정을 마치며 (15분)

**목표**: 4일간의 성장을 체감하고, 앞으로의 방향을 제시

#### 5-1. Before & After (3분)

**Day 1 시작 전 (4일 전)**:
```
~/.claude/
└── (비어있음)
```

**Day 4 종료 후 (지금)**:
```
~/.claude/
├── CLAUDE.md                      ← Day 1: 나의 매뉴얼
├── memory/                        ← Day 1: Claude가 기억하는 나
├── skills/                        ← Day 2: 나만의 워크플로우
├── agents/                        ← Day 3: 나만의 AI 팀
└── plugins/                       ← Day 4: 다른 사람의 도구까지
    └── marketplaces/
        ├── claude-code-plugins/   ← 전사 마켓플레이스
        └── omc/                   ← oh-my-claudecode
```

> "4일 전에는 빈 터미널이었는데, 지금은 여러분만의 AI 작업 환경이 구축되어 있습니다."

#### 5-2. 4일 여정 돌아보기 (2분)

| Day | 질문 | 배운 것 | 만든 것 |
|-----|------|---------|---------|
| Day 1 | "Claude가 나를 알게 하려면?" | CLAUDE.md + Memory | 나만의 Claude 세팅 |
| Day 2 | "반복 업무를 자동화하려면?" | Skill 만들기 | 나만의 워크플로우 |
| Day 3 | "혼자 못하는 일을 시키려면?" | Agent + 병렬 작업 | 나만의 AI 팀 |
| Day 4 | "이걸 동료도 쓰게 하려면?" | Plugin + Agent SDK | 다른 사람도 쓰는 도구 |

핵심 성장 경로:
```
나를 위한 세팅 → 나의 워크플로우 → 나의 AI 팀 → 모두의 도구
   (개인)          (자동화)         (위임)        (확산)
```

#### 5-3. 수강생 회고 (5분)

**AskUserQuestion으로 진행**:

> **4일간의 강의 중 가장 유용했던 것은 무엇인가요?**
>
> A) CLAUDE.md — 나만의 매뉴얼을 만든 것
> B) Skill — 반복 업무를 자동화한 것
> C) Agent — AI 팀을 구성한 것
> D) Plugin — 다른 사람과 공유할 수 있게 된 것
> E) 기타 (자유 응답)

(2~3명의 답변을 들은 후 정리)

> **여러분이 가장 기대되는 다음 스텝은?**
>
> A) 팀에 CLAUDE.md 공유하기
> B) 업무 자동화 스킬 만들기
> C) 마켓플레이스에 플러그인 배포하기
> D) 동료에게 Claude Code 소개하기

#### 5-4. 앞으로의 액션 플랜 (3분)

| 시점 | 액션 |
|------|------|
| **이번 주** | CLAUDE.md에 규칙 3가지 추가, 자주 하는 업무 1개를 Skill로 만들기 |
| **2주 내** | 팀원에게 CLAUDE.md와 Skill 공유, 피드백 받기 |
| **1개월** | 플러그인으로 패키징해서 전사 마켓플레이스에 PR 보내기 |
| **그 이후** | "AI 에이전트가 쓸 수 있는 도구"를 만드는 관점으로 업무 자동화 설계 |

#### 5-5. 마무리 메시지 (2분)

> **여러분은 이제 AI 에이전트의 도구를 만드는 사람입니다.**
>
> 4일 전에는 Claude Code가 무엇인지 몰랐던 여러분이,
> 지금은 자신만의 스킬을 만들고, 에이전트를 조합하고,
> 플러그인으로 패키징해서 동료에게 공유할 수 있게 되었습니다.
>
> 여러분이 만든 하나의 스킬이, 동료의 하루를 바꿀 수 있습니다.
> 그리고 그 스킬이 또 다른 에이전트에게 사용되어, 더 큰 일을 해낼 수 있습니다.
>
> **만들고, 공유하고, 확장하세요.**

#### 최종 퀴즈 (use camp-quiz)

> **Q1. 플러그인을 로컬에서 테스트하는 방법은?**
>
> A) claude plugin test ~/my-plugin
> B) claude --plugin-dir ~/my-plugin
> C) claude plugin install --local ~/my-plugin
>
> **정답**: B

> **Q2. 4일간 배운 것의 올바른 순서는?**
>
> A) Skill → CLAUDE.md → Plugin → Agent
> B) CLAUDE.md → Skill → Agent → Plugin
> C) CLAUDE.md → Agent → Skill → Plugin
>
> **정답**: B — 세팅 → 자동화 → 위임 → 확산

> **Q3. "딱 하나만 만들 수 있다면" 우선순위는?**
>
> A) 웹 대시보드
> B) Slack bot
> C) Skill
>
> **정답**: C — Skill을 먼저 만들면, 그 위에 GUI든 Bot이든 얹을 수 있다.

---

## 진행 규칙

### 공유 스킬 사용
- `use camp-lecture-start` — 섹션 0 시작 전 인사, OS 감지, 화면 분할 안내
- `use camp-os-detect` — OS별 명령어 분기 (macOS/Linux/Windows)
- `use camp-quiz` — 퀴즈는 한 문제씩, 답변 후 정답 공개
- `use camp-section-transition` — 섹션 간 전환 시 경과/남은 시간 표시

### 시간 관리
- **70분 경과 시**: 남은 시간 체크. 20분 미만이면 섹션 4를 5분으로 축소 (데모만)
- **80분 경과 시**: 진행 중인 섹션 마무리 후 바로 섹션 5(Wrap-up)로 이동
- **섹션 5 도달 보장**: 어떤 상황에서도 Wrap-up은 반드시 진행

### 실습 대응
- **비개발자 지원**: JSON, CLI 명령어는 Claude에게 시키기 패턴으로 진행. 직접 작성하지 않음.
- **실습 막힘 시**: 2회 시도 후 해결 안 되면 완성된 템플릿 제공
- **에러 복구**: `claude plugin validate`로 구조 검증, 오류 시 Claude에게 수정 요청
- **mari 데모 실패 시**: 사전 녹화 영상 또는 스크린샷으로 대체

### 퀴즈 규칙
- 한 문제씩 출제, 수강생 답변 후 정답 공개
- 정답 시 칭찬 + 보충 설명, 오답 시 부드러운 교정 + 이유 설명
- 퀴즈 루프 방지: 같은 문제 재출제하지 않음, 오답이어도 다음으로 진행

---

## 준비물 체크리스트

### 강사 준비
- [ ] 전사 마켓플레이스 접근 확인 (myrealtrip/claude-code-plugins)
- [ ] mari 데모 시나리오 준비 + 01_협업_ai_lab 채널 접근
- [ ] mari 데모 실패 대비 백업 (녹화 영상 또는 스크린샷)
- [ ] 스킬 배포 지원자 사전 확보 (또는 강사 본인이 시연)

### 수강생 준비
- [ ] Claude Code 설치 완료 (Day 1에서 완료)
- [ ] Day 2에서 만든 스킬 파일 존재 확인
- [ ] Slack `01_협업_ai_lab` 채널 접근 가능
- [ ] GitHub 계정 (마켓플레이스 PR용 — 강의 후 숙제)
