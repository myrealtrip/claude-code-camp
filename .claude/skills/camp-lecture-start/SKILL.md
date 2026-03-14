---
name: camp-lecture-start
description: "Claude Code Camp 강의 시작 시 공통으로 실행하는 루틴 — 인사, OS 감지, 화면 분할 안내, 준비물 확인. Use when starting any camp lecture, initializing training session, or preparing new class."
---

# camp-lecture-start: 강의 시작 공통 루틴

이 스킬은 모든 Claude Code Camp 강의(Day 1, Day 2, Day 3 등) 시작 시 공통으로 실행합니다.

## 진행 순서

1. **인사 및 강의 개요 소개**:
   - 간단히 인사한다
   - 오늘 강의의 제목, 목표, 총 소요 시간을 소개한다
   - 섹션 구성을 간략히 안내한다

2. **화면 분할 안내**: 실습 효율을 위해 터미널 창을 **좌우로 나란히** 띄우라고 안내한다:
   - **왼쪽**: 이 강의를 진행하는 Claude Code (교안 보기용)
   - **오른쪽**: 실습용 터미널 (직접 명령어를 입력하는 곳)
   - **macOS**: 창 상단 초록 버튼 길게 누르면 화면 분할 가능
   - **Windows**: Win+왼쪽/오른쪽 화살표로 창 분할 가능
   - **Linux**: 창 관리자에 따라 다르며, 터미널 탭/창을 하나 더 열어서 나란히 배치
   - 또는 터미널 탭/창을 하나 더 열어서 나란히 배치

3. **OS 자동 감지** — `use camp-os-detect`를 실행한다:
   - Bash 도구로 `uname -s` 실행
   - Darwin → macOS, Linux → Linux, 실패/그 외 → AskUserQuestion으로 직접 물어본다
   - 감지 결과를 사용자에게 알려주고, 이후 해당 OS에 맞는 안내만 제공한다

4. **준비물 확인**:
   - 강의별로 필요한 준비물을 AskUserQuestion으로 확인한다
   - 예: Day 1 — 터미널, 기본 명령어 / Day 2 — Day 1 이수, skill-creator 플러그인

5. **섹션 0부터 순서대로 진행**한다
