# Day 3 Journey Diagram — Design Spec

## Overview

Day 3 여정 다이어그램은 "Agent와 병렬 작업 — 나만의 AI 팀 만들기" 강의의 4단계 여정을 시각화합니다.
Day 2 다이어그램의 디자인 시스템을 완전히 계승하되, Agent/병렬/워크플로 개념을 새로운 레이어로 표현합니다.

---

## 1. Color Palette

### Inherited from Day 2 (unchanged)

| Token | Hex | Usage |
|-------|-----|-------|
| `bg-page` | `#0f1117` | Page background, fog overlay |
| `bg-canvas` | `#161b27` | SVG canvas background |
| `bg-canvas-border` | `#1e293b` | Canvas border |
| `color-purple` | `#a78bfa` | Claude Code node, primary accent, active states |
| `color-purple-dark` | `#4c1d95` | Message bar border |
| `color-green` | `#34d399` | Native Tool nodes |
| `color-pink` | `#f472b6` | MCP nodes |
| `color-yellow` | `#fbbf24` | Skill nodes |
| `color-orange` | `#fb923c` | Orchestration/workflow container |
| `color-blue` | `#38bdf8` | Person (사용자) node |
| `color-slate-dim` | `#64748b` | Sublabels, inactive text |
| `color-slate-mid` | `#94a3b8` | Nav button text |
| `color-slate-border` | `#334155` | Button borders, progress dots (inactive) |
| `text-primary` | `#e2e8f0` | Node labels |

### New for Day 3

| Token | Hex | Usage |
|-------|-----|-------|
| `color-agent` | `#60a5fa` | Agent nodes (blue — distinct from Tool green and MCP pink) |
| `bg-agent` | `#0f1e2e` | Agent node fill |
| `color-task` | `#a3e635` | Task nodes (lime green — suggests "done/tracked") |
| `bg-task` | `#0f1a0a` | Task node fill |
| `color-concept-fog` | `#1e293b` | Concept fog overlay tint |
| `color-parallel-glow` | `#60a5fa` | Glow effect on parallel agents |

---

## 2. Typography & Shared Styles

Identical to Day 2:

- **Font**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- **node-label**: `font-size: 11px; font-weight: 600; fill: #e2e8f0; text-anchor: middle; dominant-baseline: middle`
- **node-sublabel**: `font-size: 9px; fill: #64748b; text-anchor: middle; dominant-baseline: middle`
- **Arrow stroke-width**: `1.8` (dashed for tool/MCP calls), `2.2` for orchestration
- **Transition**: `opacity 0.5s` on nodes and arrows

---

## 3. Layout Constants

```
SVG viewBox: 0 0 1100 560

Person node:       cx=70, cy=280
Claude Code node:  x=220, y=255, w=130, h=54   (centered at y=282)
```

The right portion of the canvas is divided per stage:
- Stage 0: concept nodes (centered, foggy)
- Stage 1: single Agent column
- Stage 2: parallel Agent columns (side by side)
- Stage 3: workflow container with dependency arrows

---

## 4. Stage-by-Stage Specification

---

### Stage 0 — Concept

**Stage nav label**: `0. Concept`
**Stage bottom label**: `0. 개념: Skill / Agent / Task`

#### Message Bar Text
```
Skill은 레시피, Agent는 셰프, Task는 주문 전표 — 세 개념의 차이를 먼저 이해합니다.
```

#### Layout & Nodes

| Node ID | Type | Position (x, y, w, h) | Color | Label | Sublabel | Initial Opacity |
|---------|------|------------------------|-------|-------|---------|-----------------|
| `nodePerson` | circle cx=70 cy=280 r=22 | — | stroke `#38bdf8` | 사용자 | — | 1 |
| `nodeClaude` | rect | x=220, y=255, w=130, h=54 | stroke `#a78bfa` | Claude Code | AI 에이전트 | 1 |
| `nodeConceptSkill` | rect | x=450, y=180, w=110, h=50 | stroke `#fbbf24` fill `#1e1a0e` | Skill | 레시피 | 0 (fades in) |
| `nodeConceptAgent` | rect | x=450, y=270, w=110, h=50 | stroke `#60a5fa` fill `#0f1e2e` | Agent | 셰프 | 0 (fades in) |
| `nodeConceptTask` | rect | x=450, y=360, w=110, h=50 | stroke `#a3e635` fill `#0f1a0a` | Task | 주문 전표 | 0 (fades in) |

**Fog Layer** (`fogConceptNodes`):
- Rect covering concept nodes area: x=430, y=160, w=160, h=270
- Fill `#0f1117`, opacity=1 initially
- Contains a large `❓` text at center (cx=510, cy=295)
- Fades out on this stage (opacity → 0, revealing concept nodes beneath)

#### Arrows

| Arrow ID | From → To | Style | Opacity |
|----------|-----------|-------|---------|
| `arrPersonClaude` | Person → Claude | `arrow-main` purple, bidirectional | 1 (always visible) |
| `arrClaudePerson` | Claude → Person | `arrow-main` purple | 1 (always visible) |

No arrows to concept nodes — they appear as floating labels, not connected.

#### Fog / Visibility State

```
fogConceptNodes: 0   (concept nodes revealed)
fogStage1:       1   (stage 1 content hidden)
fogStage2:       1
fogStage3:       1
show: [nodeConceptSkill, nodeConceptAgent, nodeConceptTask]
showArrows: []
```

---

### Stage 1 — Single Agent

**Stage nav label**: `1. Single Agent`
**Stage bottom label**: `1. Agent 하나 호출`

#### Message Bar Text
```
/channel-faq Skill이 Agent를 호출합니다. Agent는 Slack MCP로 채널을 읽고, 결과를 Claude에게 돌려줍니다.
```

#### Layout & Nodes

Concept nodes are hidden. New nodes appear:

| Node ID | Type | Position (x, y, w, h) | Color | Label | Sublabel |
|---------|------|------------------------|-------|-------|---------|
| `nodeSkill1` | rect | x=390, y=255, w=118, h=54 | stroke `#fbbf24` fill `#1e1a0e` | /channel-faq | Skill |
| `nodeAgent1` | rect | x=570, y=255, w=110, h=54 | stroke `#60a5fa` fill `#0f1e2e` | Agent | 채널 분석 |
| `nodeMcpSlack1` | rect | x=760, y=220, w=100, h=36 | stroke `#f472b6` fill `#1a1a2e` | Slack MCP | — |
| `nodeToolRead1` | rect | x=760, y=270, w=100, h=36 | stroke `#34d399` fill `#0f2420` | Read | — |
| `nodeTaskDone` | rect | x=760, y=320, w=100, h=36 | stroke `#a3e635` fill `#0f1a0a` | Task ✅ | 완료 추적 |

**Agent node visual treatment**:
- Rounded rect rx=10
- Subtle inner glow: filter `glow-blue` (feGaussianBlur stdDeviation=3, blue channel)
- Small icon text at top center: `🤖` (font-size 14)

#### Arrows

| Arrow ID | Path description | Style | Color |
|----------|-----------------|-------|-------|
| `arrClaudeSkill1` | Claude right edge → Skill left | solid | `#fbbf24` |
| `arrSkill1Agent1` | Skill right edge → Agent left | solid | `#60a5fa` |
| `arrAgent1Slack` | Agent right → Slack MCP | dashed | `#f472b6` |
| `arrAgent1Read` | Agent right → Read tool | dashed | `#34d399` |
| `arrAgent1Task` | Agent right → Task node (downward curve) | dashed | `#a3e635` |

#### Fog / Visibility State

```
fogConceptNodes: 1   (concept nodes re-hidden)
show: [nodeSkill1, nodeAgent1, nodeMcpSlack1, nodeToolRead1, nodeTaskDone]
showArrows: [arrPersonClaude, arrClaudePerson, arrClaudeSkill1, arrSkill1Agent1, arrAgent1Slack, arrAgent1Read, arrAgent1Task]
```

---

### Stage 2 — Parallel Agents

**Stage nav label**: `2. Parallel Agents`
**Stage bottom label**: `2. 여러 Agent 동시 실행`

#### Message Bar Text
```
여러 채널을 동시에 분석합니다. Agent들이 병렬로 실행되어 각자 독립적으로 Slack MCP에 접근합니다 — 하나씩 기다리는 것보다 훨씬 빠릅니다!
```

#### Layout & Nodes

Single Agent nodes are hidden. Three parallel agents appear side by side horizontally.

**Skill node** (same as Stage 1, repositioned slightly left):

| Node ID | Position (x, y, w, h) | Label |
|---------|------------------------|-------|
| `nodeSkill2` | x=370, y=255, w=118, h=54 | /channel-faq |

**Agent column** — 3 agents stacked vertically with equal spacing, center-aligned at x=580:

| Node ID | Position (x, y, w, h) | Label | Sublabel |
|---------|------------------------|-------|---------|
| `nodeAgent2a` | x=520, y=160, w=110, h=48 | Agent A | dev-support |
| `nodeAgent2b` | x=520, y=256, w=110, h=48 | Agent B | product-qa |
| `nodeAgent2c` | x=520, y=352, w=110, h=48 | Agent C | infra-help |

**Visual treatment for parallel agents**:
- All three use same blue agent style
- A subtle vertical bracket `{` drawn to the left of the agent column (x=505, y=160 to y=400) in `#60a5fa` with opacity 0.4 — suggests "all at once"
- Label `|| parallel` in small text (font-size 8, color `#60a5fa` opacity 0.6) above Agent A

**MCP nodes** — each agent gets its own Slack MCP node:

| Node ID | Position (x, y, w, h) | Label |
|---------|------------------------|-------|
| `nodeMcp2a` | x=710, y=168, w=90, h=32 | Slack MCP |
| `nodeMcp2b` | x=710, y=264, w=90, h=32 | Slack MCP |
| `nodeMcp2c` | x=710, y=360, w=90, h=32 | Slack MCP |

**Results merge node** (right side, after all agents):

| Node ID | Position (x, y, w, h) | Label | Sublabel |
|---------|------------------------|-------|---------|
| `nodeMerge2` | x=870, y=240, w=110, h=80 | 결과 수집 | merged Q&A |

Fill `#1e293b`, stroke `#a78bfa` dashed, rx=10.

#### Arrows

| Arrow ID | From → To | Style |
|----------|-----------|-------|
| `arrClaudeSkill2` | Claude → Skill2 | yellow solid |
| `arrSkill2AgentA` | Skill2 → Agent2a | blue solid, curves up |
| `arrSkill2AgentB` | Skill2 → Agent2b | blue solid, straight |
| `arrSkill2AgentC` | Skill2 → Agent2c | blue solid, curves down |
| `arrAgent2aMcp` | Agent2a → MCP2a | pink dashed |
| `arrAgent2bMcp` | Agent2b → MCP2b | pink dashed |
| `arrAgent2cMcp` | Agent2c → MCP2c | pink dashed |
| `arrAgent2aMerge` | Agent2a → Merge (curves right-down) | purple dashed |
| `arrAgent2bMerge` | Agent2b → Merge (straight right) | purple dashed |
| `arrAgent2cMerge` | Agent2c → Merge (curves right-up) | purple dashed |

#### Fog / Visibility State

```
show: [nodeSkill2, nodeAgent2a, nodeAgent2b, nodeAgent2c, nodeMcp2a, nodeMcp2b, nodeMcp2c, nodeMerge2]
showArrows: [arrPersonClaude, arrClaudePerson, arrClaudeSkill2, arrSkill2AgentA, arrSkill2AgentB, arrSkill2AgentC, arrAgent2aMcp, arrAgent2bMcp, arrAgent2cMcp, arrAgent2aMerge, arrAgent2bMerge, arrAgent2cMerge]
```

---

### Stage 3 — Workflow (DAG)

**Stage nav label**: `3. Workflow (DAG)`
**Stage bottom label**: `3. 수집 → 분석 → 생성 파이프라인`

#### Message Bar Text
```
Task Topology: 수집은 병렬로, 분석과 생성은 순차로 — 의존성 그래프(DAG)가 최적 실행 순서를 결정합니다.
```

#### Layout & Nodes

All Stage 2 nodes hidden. A single n8n-style workflow canvas container appears, similar to Day 2's Stage 4 orchestration container.

**Outer container** (`orchContainer3`):
- Rect: x=220, y=60, w=860, h=440, rx=14
- Fill: `#130f1a` (slightly purple-tinted dark, distinct from Day 2's orange-tinted)
- Stroke: `#a78bfa`, stroke-width=2, stroke-dasharray=`8 4`
- Dot grid pattern fill overlay (opacity 0.4), dot color `#a78bfa`
- Header strip height=28, fill `#a78bfa` fill-opacity=0.12
- Traffic light dots: cx=238,254,270 cy=74, colors `#a78bfa`, `#60a5fa`, `#34d399`
- Container title: `/channel-faq` in `#a78bfa` at x=290, y=79

**Phase labels** (small caps inside container):

| Label | x | y | Color |
|-------|---|---|-------|
| `COLLECT (parallel)` | 340 | 95 | `#60a5fa` opacity 0.7 |
| `ANALYZE (sequential)` | 650 | 95 | `#fbbf24` opacity 0.7 |
| `GENERATE (sequential)` | 910 | 95 | `#34d399` opacity 0.7 |

**Phase dividers**: light vertical dashed lines at x=510 and x=780, y=100 to y=490, stroke `#334155`, opacity 0.5.

**Collect phase agents** (parallel, left column, x-center=340):

| Node ID | Position (x, y, w, h) | Label | Sublabel |
|---------|------------------------|-------|---------|
| `nodeWf2a` | x=280, y=150, w=110, h=46 | Agent A | dev-support |
| `nodeWf2b` | x=280, y=240, w=110, h=46 | Agent B | product-qa |
| `nodeWf2c` | x=280, y=330, w=110, h=46 | Agent C | infra-help |

All blue agent style (`#60a5fa`, fill `#0f1e2e`).

**Analyze phase agent** (center, x-center=650):

| Node ID | Position (x, y, w, h) | Label | Sublabel |
|---------|------------------------|-------|---------|
| `nodeWfAnalyze` | x=595, y=230, w=110, h=60 | Agent | Pattern Analysis |

Style: yellow (`#fbbf24`, fill `#1e1a0e`). Larger height to suggest "synthesis".

**Generate phase agent** (right, x-center=910):

| Node ID | Position (x, y, w, h) | Label | Sublabel |
|---------|------------------------|-------|---------|
| `nodeWfGenerate` | x=855, y=230, w=110, h=60 | Agent | FAQ Doc Writer |

Style: green (`#34d399`, fill `#0f2420`).

**MCP/Tool nodes below container** (y=520):

| Node ID | Position (x, y, w, h) | Label | Color |
|---------|------------------------|-------|-------|
| `nodeWfMcpA` | x=280, y=420, w=90, h=32 | Slack MCP | pink |
| `nodeWfMcpB` | x=380, y=420, w=90, h=32 | Slack MCP | pink |
| `nodeWfMcpC` | x=480, y=420, w=90, h=32 | Slack MCP | pink |
| `nodeWfWrite` | x=855, y=420, w=110, h=32 | Write (md) | green |

#### Arrows

**Collect phase — parallel fan-out from Claude/Skill**:

| Arrow ID | Path | Style |
|----------|------|-------|
| `arrWfClaudeEntry` | Claude → container left edge (y=280) | orange solid, stroke-width=2.2 |
| `arrWfEntryA` | Container entry → Agent A (curves up) | blue solid |
| `arrWfEntryB` | Container entry → Agent B (straight) | blue solid |
| `arrWfEntryC` | Container entry → Agent C (curves down) | blue solid |

**MCP connections** (agents → MCP below):

| Arrow ID | From → To | Style |
|----------|-----------|-------|
| `arrWfAMcp` | Agent A → MCP A (straight down) | pink dashed |
| `arrWfBMcp` | Agent B → MCP B (straight down) | pink dashed |
| `arrWfCMcp` | Agent C → MCP C (straight down) | pink dashed |

**Collect → Analyze fan-in** (3 agents merge into 1 analyze agent):

| Arrow ID | Path | Style |
|----------|------|-------|
| `arrWfAAnalyze` | Agent A right → Analyze left (curves right-down) | yellow solid |
| `arrWfBAnalyze` | Agent B right → Analyze left (straight right) | yellow solid |
| `arrWfCAnalyze` | Agent C right → Analyze left (curves right-up) | yellow solid |

**Analyze → Generate** (sequential, single arrow):

| Arrow ID | Path | Style |
|----------|------|-------|
| `arrWfAnalyzeGenerate` | Analyze right → Generate left | green solid, stroke-width=2.2 |

**Generate → Write tool**:

| Arrow ID | Path | Style |
|----------|------|-------|
| `arrWfGenerateWrite` | Generate bottom → Write tool (straight down) | green dashed |

#### Fog / Visibility State

```
show: [orchContainer3, nodeWf2a, nodeWf2b, nodeWf2c, nodeWfAnalyze, nodeWfGenerate, nodeWfMcpA, nodeWfMcpB, nodeWfMcpC, nodeWfWrite]
showArrows: [arrPersonClaude, arrClaudePerson, arrWfClaudeEntry, arrWfEntryA, arrWfEntryB, arrWfEntryC, arrWfAMcp, arrWfBMcp, arrWfCMcp, arrWfAAnalyze, arrWfBAnalyze, arrWfCAnalyze, arrWfAnalyzeGenerate, arrWfGenerateWrite]
```

---

## 5. Always-Visible Elements

These elements are visible across all stages:

| Element | Description |
|---------|-------------|
| `nodePerson` | 사용자 circle, cx=70, cy=280 |
| `nodeClaude` | Claude Code rect, x=220, y=255, w=130, h=54 |
| `arrPersonClaude` | Person → Claude bidirectional arrows |
| `arrClaudePerson` | Claude → Person bidirectional arrows |

---

## 6. Arrow Style Reference

| Class | Stroke Color | Dash | Width | Marker |
|-------|-------------|------|-------|--------|
| `arrow-main` | `#a78bfa` | none | 1.8 | purple |
| `arrow-tool` | `#34d399` | 4 3 | 1.8 | green |
| `arrow-mcp` | `#f472b6` | 4 3 | 1.8 | pink |
| `arrow-skill` | `#fbbf24` | none | 1.8 | yellow |
| `arrow-agent` | `#60a5fa` | none | 1.8 | blue |
| `arrow-orch` | `#a78bfa` | none | 2.2 | purple |
| `arrow-task` | `#a3e635` | 3 2 | 1.5 | lime |
| `arrow-wf-analyze` | `#fbbf24` | none | 2.0 | yellow |
| `arrow-wf-generate` | `#34d399` | none | 2.2 | green |

---

## 7. SVG Markers Required

In addition to Day 2 markers (`ah-purple`, `ah-green`, `ah-pink`, `ah-yellow`, `ah-orange`), add:

| Marker ID | Color |
|-----------|-------|
| `ah-blue` | `#60a5fa` |
| `ah-lime` | `#a3e635` |

---

## 8. SVG Filters Required

In addition to Day 2 filters (`glow-purple`, `glow-orange`), add:

| Filter ID | Effect |
|-----------|--------|
| `glow-blue` | feGaussianBlur stdDeviation=3, blue glow for Agent nodes |

---

## 9. Stage Navigation Labels

```javascript
const navLabels = [
  "0. Concept",
  "1. Single Agent",
  "2. Parallel Agents",
  "3. Workflow (DAG)"
];
```

---

## 10. Progress Dots

4 dots total (one per stage), same style as Day 2:
- Inactive: `#334155`
- Done: `#a78bfa`
- Current: `#a78bfa` with `box-shadow: 0 0 6px #a78bfa`

---

## 11. Interaction Patterns

Identical to Day 2:

| Trigger | Action |
|---------|--------|
| `→` / `↓` key | Advance to next stage |
| `←` / `↑` key | Go to previous stage |
| Stage nav button click | Jump to any stage |
| Prev / Next button | Sequential navigation |

**Transition**: all node/arrow visibility changes via CSS `opacity` transition (`0.5s ease`).
Fog overlays use `0.6s ease` for a "lifting" feel.

**Last stage button label**: `완료! 🎉`

---

## 12. Stage Transition Summary Table

| Stage | Title (nav) | Bottom label | Concept nodes | Stage 1 nodes | Stage 2 nodes | Stage 3 container |
|-------|------------|--------------|---------------|---------------|---------------|-------------------|
| 0 | 0. Concept | 0. 개념: Skill / Agent / Task | visible | hidden | hidden | hidden |
| 1 | 1. Single Agent | 1. Agent 하나 호출 | hidden | visible | hidden | hidden |
| 2 | 2. Parallel Agents | 2. 여러 Agent 동시 실행 | hidden | hidden | visible | hidden |
| 3 | 3. Workflow (DAG) | 3. 수집 → 분석 → 생성 파이프라인 | hidden | hidden | hidden | visible |

---

## 13. Key Visual Metaphors by Stage

| Stage | Metaphor | Visual Cue |
|-------|---------|------------|
| 0 | 안개 걷히듯 드러나는 세 개념 | foggy overlay fades to reveal Skill/Agent/Task as floating labeled cards |
| 1 | 셰프 한 명에게 주문서를 건넨다 | linear left-to-right chain: Claude → Skill → Agent → MCP + Task |
| 2 | 여러 셰프를 동시에 주방으로 보낸다 | fan-out from Skill to 3 parallel agents, each with own MCP, fan-in to merge node |
| 3 | 주방 전체 운영 시스템 | n8n-style container with phase dividers, fan-in merge → sequential pipeline |
