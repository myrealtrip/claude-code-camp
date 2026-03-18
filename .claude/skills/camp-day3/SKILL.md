---
name: camp-day3
description: "Day 3: Agent와 병렬 작업 — 나만의 AI 팀 만들기 - 인터랙티브 강의 진행. Use when starting day 3 training, teaching subagents, parallel work, or agent orchestration."
---

# Day 3: Agent와 병렬 작업 — 나만의 AI 팀 만들기

이 스킬은 "Agent와 병렬 작업" 강의를 인터랙티브하게 진행합니다.

## 대상

- 개발자, 비개발 직군(마케터, 세일즈, 전략, 재무, 운영 등) 모두 포함
- Day 1(CLAUDE.md + Memory), Day 2(Skill + Tool + Chaining)를 이수한 분

## 진행 방식

강의를 섹션별로 진행합니다. 각 섹션에서:

1. 핵심 개념을 설명합니다
2. 실습이 있으면 사용자와 함께 직접 실행합니다
3. 퀴즈가 있으면 사용자에게 문제를 내고 답을 확인합니다
4. 사용자가 준비되면 다음 섹션으로 넘어갑니다

총 소요 시간: 약 90분

---

## 강의 시작

`use camp-lecture-start`

위 공통 루틴 실행 후, 아래 Day 3 전용 추가 단계를 진행한다:
4. 오늘의 여정(개념 → Agent → 병렬 → 워크플로)을 소개한다
5. 준비물(Day 2 이수, Slack MCP 선택)이 갖춰져 있는지 확인한다
6. 이후 섹션 0부터 순서대로 진행한다

---

## 섹션별 진행 가이드

### 섹션 0: Recap & 오늘의 여정 (5분)

**목표**: Day 2를 복습하고 오늘의 학습 로드맵을 이해시킨다.

**진행 순서**:

1. Day 2 핵심 30초 복습:
   - 도구(Tool) = 하나의 동작 (망치질 한 번)
   - Skill = 여러 도구를 엮은 워크플로 (조립 설명서)
   - 오케스트레이션 = Skill끼리 연결 (가구 세트 완성)

2. **여정 다이어그램을 브라우저에서 연다**:
   ```bash
   open .claude/skills/camp-day3/journey-diagram.html
   ```
   (Windows: `start`, Linux: `xdg-open`)
   다이어그램이 열리면 사용자에게 "이 화면을 브라우저에 띄워두세요. 강의 진행에 따라 각 단계를 함께 볼 겁니다!"라고 안내한다.

3. 오늘의 여정을 소개한다 (다이어그램과 함께):

   ```
   Stage 1             Stage 2              Stage 3             Stage 4
   Concept             Agent                Parallel            Workflow
   +-----------+       +-----------+        +-----------+       +-----------+
   | Skill     |       | Agent     |        | Multiple  |       | Ordered   |
   | Agent     |  ==>  | Create +  |  ==>   | Agents at |  ==>  | Multi-    |
   | Task      |       | Hands-on  |        | Once      |       | Agent     |
   | Compare   |       |           |        |           |       | Pipeline  |
   +-----------+       +-----------+        +-----------+       +-----------+

   "Team leader managing AI employees"
   ```

3. 핵심 비유: **"Day 2에서 레시피를 만들었다면, Day 3에서는 여러 셰프를 고용해서 동시에 요리시킵니다!"**

4. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 0.5: 준비물 확인 (5분)

**목표**: Slack MCP 설치 여부를 확인하고, 이후 실습 트랙을 결정한다.

**진행 순서**:

1. AskUserQuestion으로 "Day 2에서 만든 `my-first-project` 폴더가 있나요?"를 확인한다
   - 없으면 아래 명령어를 새 터미널에서 실행하라고 안내한다:
     ```bash
     mkdir my-first-project
     cd my-first-project
     git init
     ```

2. Slack MCP 설치 여부를 확인한다:
   - AskUserQuestion: "Slack MCP가 설치되어 있나요?"
   - 선택지: ["네, 설치되어 있어요", "아니요, 없어요", "잘 모르겠어요"]
   - **설치되어 있는 경우**: Claude Code에서 아래를 입력해서 연결을 확인하라고 안내한다:
     ```
     내가 접근 가능한 Slack 채널 목록을 보여줘
     ```
   - **설치 안 된 경우**: 간단한 설치 안내를 해주거나, WebSearch 트랙으로 전환한다
   - **잘 모르는 경우**: 위 확인 명령어를 실행해보라고 안내한다

3. **트랙 결정**:
   - Slack MCP 있음 → **Slack 트랙** (채널 Q&A 분석)
   - Slack MCP 없음 → **WebSearch 트랙** (웹 리서치 분석)
   - "두 트랙 모두 같은 개념을 배우며, 결과물만 다릅니다. 걱정하지 마세요!"

4. AskUserQuestion으로 다음 섹션 진행 여부를 확인한다

---

### 섹션 1: Skill vs Agent vs Task (10분)

**목표**: Skill, Agent, Task 세 가지 핵심 개념을 명확히 구분한다.

> 이 섹션은 **개념 이해**에만 집중합니다. Agent의 세부 파라미터는 섹션 2에서 실습과 함께 다룹니다.

**진행 순서**:

1. **세 가지 개념 비교 테이블**을 보여준다:

   | | Skill | Agent | Task |
   |--|-------|-------|------|
   | **비유** | 레시피 📋 | 셰프 👤 | 주문 전표 ✅ |
   | **하는 일** | 재사용 가능한 워크플로 | 작업을 위임받아 실행 | 진행 상황을 추적 |
   | **실행 방법** | `/명령어`로 실행 | Skill 안에서 호출 | TaskCreate로 생성 |
   | **Day 2 예시** | `/morning-briefing` | (아직 안 배움) | (아직 안 배움) |
   | **Day 3 예시** | `/channel-faq` | "이 채널 분석해줘" | "채널 분석 완료 ✅" |

2. **핵심 비유를 확장**한다:
   - Skill = 레시피: "김치찌개 만드는 법" — 한 번 써두면 누구든 따라할 수 있다
   - Agent = 셰프: "레시피대로 요리하는 직원" — 시키면 알아서 해온다
   - Task = 주문 전표: "김치찌개 1개 — 조리 중 / 완료" — 진행 상황을 추적한다
   - **"레시피(Skill) 하나로 여러 셰프(Agent)를 시킬 수 있고, 주문 전표(Task)로 진행 상황을 관리합니다"**

3. **핵심 제약 1가지**를 짚어준다:
   > "셰프(Agent)는 다른 셰프를 고용할 수 없습니다 — 반드시 팀장(당신 또는 Skill)이 직접 고용해야 합니다."

4. **퀴즈** — `use camp-quiz` 패턴으로 **하나씩** 출제한다:

   **Q1. "매주 이메일을 요약하는 자동화"를 만들려면 어떤 것을 만들어야 하나요?**
   - A) Agent — 작업을 실행하는 직원
   - B) Skill — 재사용 가능한 워크플로
   - C) Task — 진행 상황 추적
   - 정답: **B** — Skill은 반복적으로 재사용할 수 있는 워크플로입니다. Agent는 Skill 안에서 작업을 수행하는 역할이고, Task는 진행 상황을 추적하는 도구입니다.

   **Q2. Agent가 할 수 없는 것은?**
   - A) 파일을 읽고 분석하기
   - B) 다른 Agent를 만들어서 일 시키기
   - C) 웹 검색하기
   - 정답: **B** — Agent(subagent)는 다른 Agent를 만들 수 없습니다(nesting 불가). 반드시 상위에서 직접 만들어야 합니다.

5. **Task 미니 체험** — Task가 어떻게 동작하는지 직접 보여준다:
   - "Task는 주문 전표라고 했죠? Claude Code에 이렇게 말해보세요:"
   ```
   오늘 할 일 3가지를 Task로 만들어줘:
   1. 채널 FAQ Skill 만들기
   2. 병렬 분석으로 확장하기
   3. 워크플로 완성하기
   ```
   - Claude가 `TaskCreate`로 3개 Task를 만들고, `TaskList`로 목록을 보여주는 것을 확인
   - "이렇게 Task는 **진행 상황을 추적하는 체크리스트**입니다. Agent가 작업을 끝내면 `TaskUpdate`로 완료 표시를 합니다."
   - **핵심 도구 3가지**: `TaskCreate`(생성), `TaskUpdate`(상태 변경), `TaskList`(목록 확인)
   - "이제 실습을 진행하면서 이 Task들을 하나씩 완료해볼 거예요!"

6. `use camp-section-transition`

---

### 섹션 2: Agent 만들기 (20분)

**목표**: Agent tool의 핵심 파라미터를 이해하고, 직접 Agent를 호출하는 Skill을 만든다.

> 공식 문서: https://code.claude.com/docs/en/sub-agents

**진행 순서**:

#### Agent tool 파라미터 설명 (5분)

1. **Agent tool의 핵심 파라미터** 5가지를 소개한다:

   | 파라미터 | 역할 | 비유 |
   |---------|------|------|
   | **prompt** | 업무 지시서 | "이 일을 이렇게 해줘" |
   | **subagent_type** | 직원 유형 | 탐정(Explore), 기획자(Plan), 만능(general-purpose) |
   | **model** | 직원 등급 | 인턴(haiku), 대리(sonnet), 부장(opus) |
   | **run_in_background** | 파견 방식 | 옆에서 지켜보기(false) vs 보내놓고 다른 일(true) |
   | **isolation** | 작업 공간 | 같은 사무실(기본) vs 별도 사무실(worktree) |

   - "가장 중요한 건 **prompt** — 지시를 명확하게 써야 직원이 잘 일합니다!"
   - "나머지는 상황에 따라 조절하는 옵션입니다"

2. **Built-in Agent 유형** 3가지를 소개한다:
   - **Explore** = 탐정 🔍 — 빠르고 저렴(haiku 모델), 파일을 읽기만 가능, 코드베이스 조사에 적합
   - **Plan** = 기획자 📝 — 읽기만 가능, 계획 수립에 적합
   - **general-purpose** = 만능 직원 🛠️ — 모든 도구 사용 가능, 복잡한 작업에 적합

3. **Custom Agent 정의** 방법을 간단히 소개한다:
   - `.claude/agents/` 폴더에 마크다운 파일로 정의
   - "우리 회사 전용 직원 프로필을 미리 만들어두는 것"
   - 비유: "채용 공고를 미리 작성해두고, 필요할 때 바로 셰프를 부르는 것"

AskUserQuestion으로 "여기까지 Agent의 기본 개념이 이해되셨나요?"를 확인한다.

#### 실습: 채널 분석 Agent Skill 만들기 (13분)

> **이 실습에서 만든 `channel-faq` Skill을 섹션 3, 섹션 4에서 계속 확장합니다.**

실습 시작 전에 **`my-first-project` 폴더에서 Claude Code를 실행해주세요**라고 안내한다.
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

AskUserQuestion으로 실습 난이도를 선택하게 한다:
- **가이드 모드**: 템플릿을 따라 단계별로 진행합니다
- **도전 모드**: 목표만 보고 직접 작성해봅니다

**[Slack 트랙 — 가이드 모드]**

`/skill-creator`를 입력하라고 안내한다:
```
/skill-creator
```

Claude가 인터뷰를 시작하면 아래처럼 답하라고 안내한다:
```
Slack 채널의 최근 대화를 분석해서 Q&A를 정리해주는 Skill을 만들어줘.

- AskUserQuestion으로 분석할 Slack 채널 이름을 물어봄
- Slack MCP의 slack_read_channel 도구로 해당 채널의 최근 대화를 읽어옴
- 대화 중에서 질문과 답변 패턴을 찾아서 정리
- Q&A 목록을 보기 좋은 표로 출력
- AskUserQuestion으로 추가 분석할 채널이 있는지 확인

패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 channel-faq.
```

- skill-creator가 `.claude/skills/channel-faq/SKILL.md`를 생성하는 것을 확인

**[Slack 트랙 — 도전 모드]**

"Slack MCP를 활용해서 특정 채널의 Q&A를 정리하는 Skill을 만들어보세요. 스킬 이름은 `channel-faq`입니다. `/skill-creator`를 사용하세요. 힌트: `slack_read_channel` 도구를 활용하면 됩니다."

<!-- FALLBACK: WebSearch -->
**[WebSearch 트랙 — 가이드 모드]**

`/skill-creator`를 입력하라고 안내한다:
```
/skill-creator
```

Claude가 인터뷰를 시작하면 아래처럼 답하라고 안내한다:
```
특정 주제에 대해 웹에서 조사해서 Q&A를 정리해주는 Skill을 만들어줘.

- AskUserQuestion으로 조사할 주제를 물어봄
- WebSearch 도구로 해당 주제에 대한 최신 정보를 검색
- 검색 결과에서 자주 묻는 질문과 답변 패턴을 정리
- Q&A 목록을 보기 좋은 표로 출력
- AskUserQuestion으로 추가 조사할 주제가 있는지 확인

패키징 하지 말고, 현재 디렉토리의 .claude/skills/ 에 바로 만들어줘. 스킬 이름은 channel-faq.
```

**[WebSearch 트랙 — 도전 모드]**

"WebSearch를 활용해서 특정 주제의 Q&A를 정리하는 Skill을 만들어보세요. 스킬 이름은 `channel-faq`입니다. `/skill-creator`를 사용하세요."
<!-- /FALLBACK -->

생성된 Skill을 바로 실행한다:
```
/channel-faq
```

- Slack 트랙: 채널 이름을 입력하면 Q&A가 정리되는 것을 확인
- WebSearch 트랙: 주제를 입력하면 Q&A가 정리되는 것을 확인
- **"Skill 안에서 Agent가 외부 데이터를 가져와서 분석하는 것을 봤습니다!"**

**실습 결과 검증**:
- "`.claude/skills/channel-faq/SKILL.md` 파일이 생성되었는지 확인해보세요"
- AskUserQuestion: "SKILL.md 파일이 잘 만들어졌나요?"
  - 선택지: ["네, 잘 만들어졌어요!", "파일이 안 보여요", "에러가 났어요"]
  - 안 보이거나 에러 시: 디버깅을 도와주고, 필요하면 가이드 모드 템플릿을 직접 제공한다

#### 퀴즈 (2분)

AskUserQuestion으로 퀴즈를 출제한다:

**Q. Agent의 `model` 파라미터를 `haiku`로 설정하면 어떤 효과가 있나요?**
- A) Agent가 더 똑똑해진다
- B) Agent가 더 빠르고 저렴하게 동작한다
- C) Agent가 파일을 수정할 수 없게 된다
- 정답: **B** — haiku는 가장 빠르고 저렴한 모델입니다. Explore agent가 기본으로 haiku를 쓰는 이유도 이것입니다. 모델 선택은 속도/비용/능력의 트레이드오프입니다.

사용자에게 Claude Code를 종료(`/exit`)하라고 안내한다.

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 3: 병렬 작업 (20분)

**목표**: 여러 Agent를 동시에 실행하는 병렬 패턴을 이해하고 직접 체험한다.

**진행 순서**:

#### 병렬 실행 개념 설명 (7분)

1. **병렬 실행이란?**
   - 비유: **식당 주방**
     - 셰프 1명이 파스타 끓이고 → 샐러드 만들고 → 수프 데우면 **느리다**
     - 셰프 3명이 **동시에** 하면 **빠르다!**
   - "Agent도 마찬가지 — 하나씩 시키면 순차, 여럿을 한번에 시키면 병렬"

2. **병렬 실행 패턴 2가지**:

   | 패턴 | 비유 | 사용 시점 |
   |------|------|----------|
   | 여러 Agent 동시 호출 | 셰프 3명에게 동시에 주문 | 결과를 **모아서** 써야 할 때 |
   | `run_in_background` | 셰프를 보내놓고 다른 일 | 결과가 **나중에** 와도 될 때 |

3. **병렬 vs 순차 판단 기준** — 가장 중요한 질문: **"이 작업들이 서로 독립적인가?"**
   - **독립적** → 병렬 (각 채널 분석은 서로 관련 없음 → 동시에 가능)
   - **의존적** → 순차 (분석 결과가 있어야 보고서를 쓸 수 있음 → 순서대로)

   ```
   [Independent Tasks ==> Parallel]    [Dependent Tasks ==> Sequential]
   +----------+  +----------+          +----------+    +----------+
   | Channel  |  | Channel  |          | Collect  | => | Report   |
   | A        |  | B        |          | Data     |    | Write    |
   +----------+  +----------+          +----------+    +----------+
   Run at the same time                Must wait for previous step
   ```

4. **Agent Teams 간단 언급** (실습 없음):
   > "더 복잡한 협업이 필요하면 Agent Teams라는 기능도 있습니다. 직원들끼리 직접 대화하며 협업하는 것인데, 아직 실험적 기능이라 오늘은 개념만 소개합니다."

AskUserQuestion으로 "병렬 개념이 이해되셨나요?"를 확인한다.

#### 실습: 병렬 채널 분석 Skill 만들기 (11분)

> **프로그레시브 빌딩**: 섹션 2에서 만든 `channel-faq` Skill을 확장합니다.

Claude Code를 실행하라고 안내한다:
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

AskUserQuestion으로 실습 난이도를 선택하게 한다:
- **가이드 모드**: 2개 채널 병렬 분석을 템플릿으로 따라합니다
- **도전 모드**: 4개 채널 병렬 분석을 직접 설계합니다

**[Slack 트랙 — 가이드 모드]**

```
/skill-creator
```
```
기존 channel-faq 스킬을 확장해서, 여러 Slack 채널을 동시에 분석하는 Skill을 만들어줘.

- 분석할 채널 목록: dev-support, product-qa (2개 채널)
- 각 채널에 대해 별도의 Agent를 병렬로 실행해서 Q&A를 수집
- Agent는 Slack MCP의 slack_read_channel로 채널 대화를 읽고, 질문/답변 패턴을 정리
- 모든 Agent의 결과를 모아서 채널별 Q&A 요약을 출력
- 마지막에 AskUserQuestion으로 추가 채널 분석 여부 확인

패키징 하지 말고, 기존 .claude/skills/channel-faq/SKILL.md를 덮어써줘.
```

**[Slack 트랙 — 도전 모드]**

"channel-faq 스킬을 확장해서 dev-support, product-qa, infra-help, design-feedback **4개 채널을 병렬로** 분석하도록 만들어보세요. `/skill-creator`로 기존 SKILL.md를 업데이트하세요."

<!-- FALLBACK: WebSearch -->
**[WebSearch 트랙 — 가이드 모드]**

```
/skill-creator
```
```
기존 channel-faq 스킬을 확장해서, 여러 주제를 동시에 웹 조사하는 Skill을 만들어줘.

- 조사할 주제 목록: "AI 여행 서비스 트렌드", "원격 근무 도구 추천" (2개 주제)
- 각 주제에 대해 별도의 Agent를 병렬로 실행해서 Q&A를 수집
- Agent는 WebSearch로 주제를 검색하고, 자주 묻는 질문/답변을 정리
- 모든 Agent의 결과를 모아서 주제별 Q&A 요약을 출력
- 마지막에 AskUserQuestion으로 추가 주제 조사 여부 확인

패키징 하지 말고, 기존 .claude/skills/channel-faq/SKILL.md를 덮어써줘.
```

**[WebSearch 트랙 — 도전 모드]**

"channel-faq 스킬을 확장해서 4개 주제(예: AI 여행 서비스, 원격 근무, 프로젝트 관리, 팀 커뮤니케이션)를 **병렬로** 웹 조사하도록 만들어보세요."
<!-- /FALLBACK -->

실행:
```
/channel-faq
```

- 여러 Agent가 동시에 실행되는 것을 확인하게 한다
- **"하나씩 순서대로 분석하는 것보다 동시에 돌리니까 훨씬 빠르죠? 이것이 병렬의 힘입니다!"**

**실습 결과 검증**:
- "SKILL.md가 업데이트되었는지 확인해보세요. 파일 안에 `Agent`와 `run_in_background` 같은 병렬 관련 키워드가 들어있어야 합니다."
- AskUserQuestion: "병렬 분석 Skill이 잘 동작했나요?"
  - 선택지: ["네, 여러 Agent가 동시에 돌았어요!", "하나씩 순서대로 돌았어요", "에러가 났어요"]
  - 순차 실행이었거나 에러 시: 원인을 파악하고 Agent 호출 부분을 함께 점검한다

#### 퀴즈 (2분)

AskUserQuestion으로 퀴즈를 출제한다:

**Q. 다음 중 병렬로 실행해야 하는 상황은?**
- A) 데이터를 수집한 후 → 분석 보고서를 작성
- B) 4개 부서의 주간 보고서를 각각 수집
- C) 보고서를 작성한 후 → 팀장에게 전송
- 정답: **B** — 4개 부서의 보고서 수집은 서로 **독립적**이므로 병렬로 실행할 수 있습니다. A와 C는 앞의 결과가 있어야 다음을 할 수 있는 **순차** 작업입니다. 핵심 질문을 기억하세요: "이 작업들이 서로 독립적인가?"

사용자에게 Claude Code를 종료(`/exit`)하라고 안내한다.

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 4: Task Topology — 워크플로 설계 (25분)

**목표**: 의존성이 있는 multi-agent workflow를 이해하고 Skill로 구현한다.

**진행 순서**:

#### 워크플로 개념 설명 (8분)

1. **Task Topology(작업 흐름)란?**
   - 비유: **요리 레시피의 순서**
     - 재료 손질 (양파, 당근, 고기) → **병렬 가능** (동시에 써는 게 빠름)
     - 재료를 볶다 → **순차** (손질이 끝나야 볶을 수 있음)
     - 접시에 담다 → **순차** (볶아야 담을 수 있음)
   - "병렬과 순차를 **섞어서** 최적의 흐름을 만드는 것이 Task Topology입니다"

2. **우리가 만들 워크플로 다이어그램**:

   ```
   [Collect Phase -- Parallel]       [Analyze Phase]       [Generate Phase]
   +----------------+
   | Agent(channel1) |--+
   +----------------+  |    +------------------+    +-------------------+
   | Agent(channel2) |--+--> | Pattern-Analysis |===>| FAQ Doc Generator |
   +----------------+  |    | Agent            |    | Agent             |
   | Agent(channel3) |--+    +------------------+    +-------------------+
   +----------------+
    Parallel collection          Merge and analyze        Create final docs
   ```

3. **왜 필요한가?**:
   > "섹션 3에서 병렬로 수집만 했는데, 수집한 데이터를 **분석하고 → 문서를 만드는 과정까지 자동화**하고 싶다면? 바로 이 워크플로가 필요합니다!"

4. **실생활 예시**:
   - 회의 준비: 여러 부서 자료 수집(병렬) → 요약 정리(순차) → 보고서 작성(순차)
   - 분기 보고: 매출+고객+마케팅 데이터 수집(병렬) → 통합 분석(순차) → 경영진 보고서(순차)

AskUserQuestion으로 "워크플로 개념이 이해되셨나요?"를 확인한다.

#### 실습: 워크플로 완성 (15분)

> **프로그레시브 빌딩**: 섹션 3에서 만든 병렬 분석 Skill을 최종 워크플로로 완성합니다.

Claude Code를 실행하라고 안내한다:
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

AskUserQuestion으로 실습 난이도를 선택하게 한다:
- **가이드 모드**: 수집 → 보고서 2단계 워크플로를 템플릿으로 따라합니다
- **가이드 챌린지 모드**: 수집 → 분석 → 문서 3단계 구조는 주어지되, 프롬프트는 직접 작성합니다
- **도전 모드**: 3단계 DAG 워크플로 전체를 자유롭게 설계합니다

**[Slack 트랙 — 가이드 모드]**

```
/skill-creator
```
```
기존 channel-faq 스킬을 확장해서, 채널 수집 결과를 기반으로 FAQ 문서까지 자동 생성하는 Skill을 만들어줘.

Step 1 - 수집 (병렬):
- dev-support, product-qa 채널에 각각 Agent를 병렬로 보내서 Q&A를 수집
- 각 Agent는 Slack MCP로 채널 대화를 읽고 질문/답변을 정리

Step 2 - FAQ 문서 생성 (순차):
- Step 1의 결과를 모아서, 채널별 FAQ 마크다운 문서를 Write 도구로 생성
- 파일명: faq-{채널이름}.md
- 형식: 제목, 날짜, Q&A 목록 (질문 + 답변 + 출처)

패키징 하지 말고, 기존 .claude/skills/channel-faq/SKILL.md를 덮어써줘.
```

**[Slack 트랙 — 가이드 챌린지 모드]**

"channel-faq 스킬을 **3단계**로 확장해보세요:
1. **수집**: 여러 채널을 병렬 Agent로 분석 (섹션 3에서 한 것)
2. **패턴 분석**: 수집 결과에서 공통 질문 패턴을 추출하는 Agent 추가
3. **FAQ 생성**: 패턴 분석 결과로 팀별 FAQ 문서 생성

`/skill-creator`로 만들되, 각 단계의 상세 프롬프트는 직접 작성해보세요."

**[Slack 트랙 — 도전 모드]**

"channel-faq 스킬을 4개 채널 + 3단계 DAG 워크플로로 완성해보세요. 수집(병렬) → 분석(순차) → 생성(순차) 구조를 자유롭게 설계하세요."

<!-- FALLBACK: WebSearch -->
**[WebSearch 트랙 — 가이드 모드]**

```
/skill-creator
```
```
기존 channel-faq 스킬을 확장해서, 웹 조사 결과를 기반으로 FAQ 문서까지 자동 생성하는 Skill을 만들어줘.

Step 1 - 조사 (병렬):
- "AI 여행 서비스 트렌드", "원격 근무 도구 추천" 주제에 각각 Agent를 병렬로 보내서 Q&A를 수집
- 각 Agent는 WebSearch로 검색하고 자주 묻는 질문/답변을 정리

Step 2 - FAQ 문서 생성 (순차):
- Step 1의 결과를 모아서, 주제별 FAQ 마크다운 문서를 Write 도구로 생성
- 파일명: faq-{주제}.md
- 형식: 제목, 날짜, Q&A 목록 (질문 + 답변 + 출처 URL)

패키징 하지 말고, 기존 .claude/skills/channel-faq/SKILL.md를 덮어써줘.
```

**[WebSearch 트랙 — 가이드 챌린지 모드]**

"channel-faq 스킬을 3단계로 확장: 수집(병렬 웹 조사) → 패턴 분석(공통 질문 추출) → FAQ 생성(문서 작성). `/skill-creator`로 만들되, 각 단계의 프롬프트는 직접 작성하세요."

**[WebSearch 트랙 — 도전 모드]**

"4개 주제 + 3단계 DAG 워크플로를 자유롭게 설계하세요."
<!-- /FALLBACK -->

실행:
```
/channel-faq
```

- 수집이 병렬로 돌아간 후, 순차적으로 문서가 생성되는 것을 확인하게 한다
- 생성된 FAQ 마크다운 파일을 함께 확인한다
- **"수집은 병렬로, 분석과 생성은 순차로 — 이것이 Task Topology입니다! 하나의 Skill 안에서 여러 Agent가 순서를 지키며 협업하는 것을 만들었습니다!"**

**실습 결과 검증**:
- "생성된 FAQ 파일을 확인해보세요:"
  - Slack 트랙: `faq-dev-support.md`, `faq-product-qa.md` 파일 존재 여부
  - WebSearch 트랙: `faq-ai-여행-서비스-트렌드.md` 등 주제별 파일 존재 여부
- AskUserQuestion: "FAQ 마크다운 파일이 생성되었나요?"
  - 선택지: ["네, 파일이 잘 만들어졌어요!", "파일이 없어요", "내용이 비어있어요"]
  - 문제 시: SKILL.md의 Write 도구 호출 부분과 파일 경로를 함께 점검한다

#### 퀴즈 (2분)

AskUserQuestion으로 퀴즈를 **하나씩** 출제한다:

**Q1. 다음 워크플로에서 병렬로 실행할 수 있는 단계는?**
```
A. 4개 부서 데이터 수집
B. 수집 데이터를 하나로 합치기
C. 합친 데이터로 보고서 작성
```
- A) A만 병렬 가능
- B) A와 B 병렬 가능
- C) 전부 병렬 가능
- 정답: **A** — 4개 부서 데이터 수집(A)은 서로 독립적이므로 병렬 가능. 합치기(B)는 수집이 끝나야 하고, 보고서(C)는 합치기가 끝나야 하므로 순차입니다.

**Q2. 섹션 2에서 만든 단일 Agent → 섹션 3 병렬 Agent → 섹션 4 워크플로 완성. 이 과정을 한 단어로 표현하면?**
- A) 오케스트레이션
- B) 리팩토링
- C) 디버깅
- 정답: **A** — 여러 Agent를 조율하여 하나의 워크플로를 만드는 것이 오케스트레이션입니다. Day 2에서 Skill 오케스트레이션을 배웠듯이, Day 3에서는 Agent 오케스트레이션을 배운 것입니다!

AskUserQuestion으로 다음 섹션 진행 여부를 확인한다.

---

### 섹션 5: Wrap-up — 오늘 배운 것 (5분)

**목표**: 학습 내용을 정리하고 다음 단계를 안내한다.

**진행 순서**:

1. 오늘 배운 4단계를 정리한다:

   | 단계 | 비유 | 배운 것 |
   |------|------|---------|
   | 🧠 개념 | 레시피 / 셰프 / 전표 | Skill, Agent, Task 구분 |
   | 👤 Agent | 셰프 한 명 고용 | Agent tool 파라미터, custom agent |
   | 👥 병렬 | 여러 셰프 동시에 | 병렬 실행 패턴, 판단 기준 |
   | 🔄 워크플로 | 주방 운영 시스템 | Task Topology, 의존성 그래프 |

2. 3일간의 여정을 보여준다:
   ```
   Day 1: Claude's personality      (CLAUDE.md, Memory)
          Claude의 성격을 설정했다

   Day 2: Claude's skills           (Tool -> Skill -> Orchestration)
          Claude에게 기술을 가르쳤다

   Day 3: Claude's team             (Agent -> Parallel -> Workflow)
          Claude에게 팀을 만들어줬다     <-- Today!
   ```

3. AskUserQuestion으로 종합 퀴즈를 출제한다:

   **Q. 다음 중 올바른 설명은?**
   - A) Agent는 다른 Agent를 만들 수 있다
   - B) Skill 안에서 여러 Agent를 병렬로 실행할 수 있다
   - C) Task는 Agent와 같은 것이다
   - 정답: **B** — Skill 안에서 여러 Agent를 동시에(병렬로) 실행할 수 있습니다. Agent는 다른 Agent를 만들 수 없고(nesting 불가), Task는 진행 추적 도구로 Agent와는 다른 역할입니다.

4. 다음 단계를 안내한다:
   - 만든 `channel-faq` Skill을 실제 업무에서 써보기
   - 다른 채널이나 데이터 소스로 확장해보기
   - "어떤 반복 작업을 병렬화할 수 있을까?" 목록 만들어보기
   - 동료에게 `/channel-faq`를 공유해보기

---

## 진행 규칙

- **퀴즈 진행**: `use camp-quiz` 패턴을 따르세요 — AskUserQuestion으로 선택지 출제, 사용자 답변 후 정답 공개, 맞으면 칭찬/틀리면 해설.
- **섹션 전환**: `use camp-section-transition` 패턴을 따르세요 — 섹션 핵심 요약 후 남은 시간과 함께 다음 섹션 진행 여부를 확인. 각 섹션별 누적 시간 기준:
  - 섹션 0 끝 (5분 경과) → "남은 시간: 약 85분"
  - 섹션 0.5 끝 (10분 경과) → "남은 시간: 약 80분"
  - 섹션 1 끝 (20분 경과) → "남은 시간: 약 70분"
  - 섹션 2 끝 (40분 경과) → "남은 시간: 약 50분"
  - 섹션 3 끝 (60분 경과) → "남은 시간: 약 30분"
  - 섹션 4 끝 (85분 경과) → "남은 시간: 약 5분"
- **실습 난이도 분기**: 모든 실습에서 AskUserQuestion으로 "가이드 모드 / 도전 모드"를 물어본다. 섹션 4에서는 "가이드 챌린지 모드"도 추가한다.
- **실습 트랙 분기**: 섹션 0.5에서 결정한 트랙(Slack/WebSearch)을 이후 모든 실습에 일관되게 적용한다. `<!-- FALLBACK: WebSearch -->` 블록은 WebSearch 트랙 전용 내용이다.
- **프로그레시브 빌딩**: 섹션 2에서 만든 `channel-faq` Skill 파일을 섹션 3, 섹션 4에서 계속 확장한다. 각 섹션의 `/skill-creator` 프롬프트에 "기존 SKILL.md를 덮어써줘"로 안내한다.
- **Claude Code 재시작**: 섹션 2 실습 후, 섹션 3 실습 후 Claude Code를 종료(`/exit`)하라고 안내한다. 이는 context 관리를 위한 것이다.
- **시간 부족 시**: 남은 시간이 예상보다 적으면, 가이드 모드로 통일하거나 섹션 4를 개념 소개 + 데모로 축소한다. AskUserQuestion으로 진행 방식을 확인한다.
- **섹션 5 도달 보장**: 섹션 3 완료 시점에서 반드시 남은 시간을 체크한다. 남은 시간이 15분 미만이면 섹션 4를 "개념 설명(5분) + 데모 시연(5분)"으로 축소하고, 반드시 섹션 5 Wrap-up(5분)까지 진행한다. 섹션 4 실습 중이라도 85분 경과 시 실습을 중단하고 "나머지는 강의 후 직접 해보세요"로 마무리한 뒤 섹션 5로 전환한다. **섹션 5 Wrap-up은 절대 생략하지 않는다.**
- 실습 단계에서는 사용자가 **별도의 터미널**에서 직접 명령어를 실행하도록 안내하되, 막히면 도와주세요.
- 사용자가 질문하면 교안 내용을 기반으로 답변하세요.
- 교안에 없는 내용이면 솔직히 말하고, 관련 레퍼런스를 안내하세요.
- 설명할 때는 비유를 적극 활용하세요. 교안에 있는 비유(레시피/셰프/전표, 식당 주방 등)를 우선 사용하세요.
- (비개발자인 경우에만) 기술 용어는 항상 비유와 함께 설명하세요.
- `/skill-creator` 실습에서는 "패키징 하지 말고, 현재 디렉토리에 바로 만들어줘"를 안내하세요.
