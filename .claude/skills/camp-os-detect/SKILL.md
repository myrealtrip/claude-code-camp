---
name: camp-os-detect
description: "강의 중 사용자의 OS를 자동 감지하고, 이후 해당 OS에 맞는 안내를 제공하기 위한 공통 루틴. Use when detecting user OS, providing OS-specific instructions, or at lecture start."
---

# camp-os-detect: OS 자동 감지

이 스킬은 강의 시작 시 사용자의 OS를 자동으로 감지합니다.
`camp-lecture-start` 등 다른 스킬에서 `use camp-os-detect` 형태로 호출하거나, 단독으로 실행할 수 있습니다.

## 진행 순서

1. **Bash 도구로 `uname -s`를 실행**한다:
   ```bash
   uname -s
   ```

2. **결과에 따라 OS를 판별**한다:
   - `Darwin` → **macOS** (터미널 앱 사용)
   - `Linux` → **Linux** (터미널 앱 사용)
   - 명령어 실행 실패 또는 그 외 결과 → AskUserQuestion으로 "현재 사용 중인 OS가 macOS, Windows, Linux 중 어느 것인가요?"를 직접 물어본다

3. **감지 결과를 사용자에게 알려준다**: 예: "macOS를 사용 중이시군요! 이후 안내는 macOS 기준으로 드리겠습니다."

4. **이후 강의 전체에서 해당 OS에 맞는 안내만 제공**한다:
   - macOS/Linux: `brew`, 터미널 앱, `~/.claude/` 경로 등 사용
   - Windows: `%USERPROFILE%\.claude\` 경로, PowerShell, winget/npm 등 사용

## 반환 값

이 스킬 실행 후 다음 정보를 컨텍스트에 유지한다:
- 감지된 OS: `macOS` / `Linux` / `Windows`
- 이후 모든 경로, 명령어, 설치 안내를 해당 OS 기준으로 제공
