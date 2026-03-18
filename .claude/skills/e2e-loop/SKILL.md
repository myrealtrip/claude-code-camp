---
name: e2e-loop
description: |
  E2E 테스트 → 분석 → SKILL.md 수정 → 커밋을 반복하는 자동 개선 루프.
  /e2e-test 를 내부적으로 호출하여 테스트하고, 9점 미만 criterion을 자동 수정.
  트리거 - "e2e loop", "e2e-loop", "자동 개선", "반복 테스트",
  "테스트 루프", "품질 루프", "quality loop"
---

# E2E Loop — 자동 테스트 + 개선 반복 스킬

E2E 테스트 → 결과 분석 → SKILL.md 수정 → 커밋을 **자동 반복**하여, 모든 평가 기준이 목표 점수 이상이 될 때까지 개선합니다.

## 인자 형식

```
/e2e-loop --dir <test-dir> --skill <skill-path> --prompt "<initial-prompt>" [--target-score 9] [--max-iterations 10] [--exclude <criterion-ids>]
```

| 인자 | 필수 | 기본값 | 설명 |
|------|------|--------|------|
| `--dir` | O | — | 테스트 디렉토리. 이 안에서 `*personas*` 와 `*criteria*` YAML을 자동 탐색 |
| `--skill` | O | — | 수정 대상 SKILL.md 경로 (예: `.claude/skills/camp-day3/SKILL.md`) |
| `--prompt` | O | — | 초기 프롬프트 (e2e-test에 전달) |
| `--target-score` | X | 9 | 모든 criterion이 이 점수 이상이면 종료 |
| `--max-iterations` | X | 10 | 최대 반복 횟수 |
| `--exclude` | X | — | 수정 대상에서 제외할 criterion ID (쉼표 구분) |

### 디렉토리 자동 탐색 규칙

`--dir` 안에서 Glob으로 파일을 찾습니다:
- **personas**: `*persona*` 패턴에 매칭되는 `.yaml` 또는 `.yml` 파일 (예: `beta-tester-personas.yaml`)
- **criteria**: `*criteria*` 또는 `*evaluation*` 패턴에 매칭되는 `.yaml` 또는 `.yml` 파일 (예: `evaluation-criteria.yaml`)

탐색 실패 시 에러를 출력하고 종료합니다. 여러 파일이 매칭되면 첫 번째를 사용하고 경고를 출력합니다.

**예시:**
```
/e2e-loop --dir day3 --skill .claude/skills/camp-day3/SKILL.md --prompt "/camp-day3" --exclude error_handling
```
→ `day3/beta-tester-personas.yaml`과 `day3/evaluation-criteria.yaml`을 자동으로 찾아서 사용

## 실행 흐름

```
┌─────────────────────────────────────────────────┐
│                  E2E Loop                        │
│                                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐     │
│  │ 1. Test  │──>│ 2. Analyze│──>│ 3. Fix   │     │
│  │ /e2e-test│   │ Reports  │   │ SKILL.md │     │
│  └──────────┘   └──────────┘   └──────────┘     │
│       ^                             │            │
│       │         ┌──────────┐        │            │
│       └─────────│ 4. Commit│<───────┘            │
│                 │ + Check  │                     │
│                 └──────────┘                     │
│                      │                           │
│              All >= target? ──Yes──> Done!        │
│                      │                           │
│                     No                           │
│                      │                           │
│              iteration < max? ──No──> Stop        │
│                      │                           │
│                    Yes ──> Loop back to 1         │
└─────────────────────────────────────────────────┘
```

## Step 1: E2E 테스트 실행

기존 리포트를 삭제하고 `/e2e-test`를 호출합니다.

### 1-1. Run ID 생성 + 리포트 디렉토리 준비

매 iteration마다 **6자리 UID**를 생성하여 리포트 파일명에 포함합니다. 이전 리포트와 충돌을 방지합니다.

```bash
# 6자리 UID 생성
RUN_ID=$(python3 -c "import uuid; print(uuid.uuid4().hex[:6])")
echo "Run ID: $RUN_ID"
```

리포트 경로: `.reports/{RUN_ID}/report-persona-{id}.md`, `.reports/{RUN_ID}/summary.md`

```bash
mkdir -p .reports/$RUN_ID
```

이전 iteration의 리포트가 다른 디렉토리에 있으므로 teammate가 재사용할 위험이 없습니다.

### 1-2. E2E 테스트 실행

`/e2e-test --personas {자동탐색된_personas_파일} --criteria {자동탐색된_criteria_파일} --prompt "{prompt}"` 를 실행합니다.

**중요**: e2e-test teammate에게 리포트 출력 경로를 `.reports/{RUN_ID}/` 로 지정하세요. 이전 iteration 리포트와 분리하기 위해 반드시 RUN_ID 디렉토리를 사용합니다.

이 스킬의 동작 방식대로:
- TeamCreate로 페르소나별 teammate 생성
- 각 teammate가 `claude -p` multi-turn 대화 실행
- LLM-as-Judge 평가
- per-persona 리포트 생성

**모든 teammate 완료를 대기**합니다.

## Step 2: 결과 분석

### 2-1. 리포트 수집

모든 `.reports/{RUN_ID}/report-persona-*.md` 파일을 읽어서 각 criterion별 점수를 추출합니다.

### 2-2. 점수 매트릭스 생성

```
| Criterion             | P1  | P2  | P3  | P4  | P5  | Min | Avg |
|-----------------------|-----|-----|-----|-----|-----|-----|-----|
| learning_overview     | 9   | 8   | 9   | 9   | 9   | 8   | 8.8 |
| difficulty_fit        | 9   | 8   | 8   | 9   | 9   | 8   | 8.6 |
| ...                   |     |     |     |     |     |     |     |
```

### 2-3. 목표 달성 확인

- `--exclude`에 지정된 criterion은 체크에서 제외
- **모든 페르소나의 모든 criterion(제외 항목 외)**이 `--target-score` 이상이면 → **성공, 루프 종료**
- 하나라도 미달이면 → Step 3으로

### 2-4. 개선 필요 항목 식별

target-score 미만인 criterion들을 수집:
- criterion ID, 이름
- 미달 페르소나와 점수
- 해당 criterion의 suggestion 목록 (리포트에서 추출)

## Step 3: SKILL.md 수정

### 3-1. 수정 계획 수립

개선 필요 항목을 분석하여 SKILL.md의 어느 부분을 수정할지 계획합니다:

- **suggestion 기반**: 리포트의 suggestion에서 actionable한 개선안 추출
- **패턴 분석**: 여러 페르소나에서 공통으로 낮은 점수의 criterion → 구조적 문제
- **단일 페르소나 문제**: 한 페르소나만 낮은 점수 → 적응적 대응 부족

### 3-2. SKILL.md 수정 실행

`--skill` 경로의 SKILL.md 파일을 `Read`하고 `Edit`으로 수정합니다.

**수정 원칙**:
- 기존 구조를 최대한 유지하고, 부족한 부분만 보강
- 지시문을 더 명시적으로 만들어 Claude가 의도대로 동작하도록
- 새 콘텐츠를 추가할 때는 기존 톤과 스타일을 유지
- 한 iteration에 너무 많이 바꾸지 않음 (3-5개 Edit 이내)

### 3-3. 수정 사항 요약

콘솔에 수정 내용을 출력:
```
📝 Iteration {n} 수정 사항:
  1. [criterion_id] 수정 내용 요약
  2. [criterion_id] 수정 내용 요약
  ...
```

## Step 4: 커밋 + 체크

### 4-1. 커밋

```bash
git add {skill_path}
git commit -m "fix: E2E loop iteration {n} — {수정_요약}

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>"
```

### 4-2. 반복 판단

- iteration < max_iterations → Step 1로 돌아감
- iteration >= max_iterations → 최종 결과 출력 후 종료

## Step 5: 최종 출력

### 성공 시
```
🎉 E2E Loop Complete! (Iteration {n}/{max})
   Target: {target_score}/10 — ALL CRITERIA MET

   Iteration History:
   | Iter | Min Score | Avg Score | Fixes |
   |------|-----------|-----------|-------|
   | 1    | 7.3       | 8.04      | 6     |
   | 2    | 8.5       | 8.72      | 3     |
   | 3    | 9.0       | 9.15      | 0     |

   Final Reports: .reports/summary.md
```

### max-iterations 도달 시
```
⚠️ E2E Loop Stopped (max iterations reached: {max})
   Target: {target_score}/10 — NOT MET

   Remaining below target:
   | Criterion        | Min Score | Personas |
   |------------------|-----------|----------|
   | error_handling   | 8         | p1, p3   |

   Iteration History:
   ...

   Final Reports: .reports/summary.md
```

## Notes

- 각 iteration에서 `/e2e-test`를 호출하므로, e2e-test의 모든 기능(페르소나 시뮬레이션, LLM-as-Judge 등)을 그대로 사용
- 리포트 파일은 매 iteration마다 덮어쓰므로, 이전 iteration 리포트는 보존되지 않음 (git history로 추적 가능)
- `--exclude`는 구조적으로 수정 불가능한 criterion(예: AskUserQuestion 관련)을 제외할 때 사용
- 비용 주의: iteration당 ~$5-10 (5 personas × $1-2 each), max 10회면 최대 ~$100
