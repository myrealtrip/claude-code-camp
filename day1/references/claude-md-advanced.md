# CLAUDE.md 심화

> Day 1에서 다루지 않은 CLAUDE.md의 고급 기능들

## .claude/CLAUDE.md vs ./CLAUDE.md

프로젝트 CLAUDE.md는 두 곳 중 하나에 둘 수 있습니다:

| 위치 | 설명 |
|------|------|
| `./CLAUDE.md` | 프로젝트 루트에 바로 보임 |
| `./.claude/CLAUDE.md` | `.claude/` 폴더 안에 정리 |

둘 다 같은 scope(프로젝트 레벨)이고 기능 차이 없습니다. `.claude/` 폴더에 넣으면 프로젝트 루트가 깔끔해지는 장점이 있습니다.

> `.claude/` 폴더의 git 추적 여부는 프로젝트마다 다를 수 있으므로, 팀과 공유가 확실히 필요하면 `./CLAUDE.md`를 사용하는 것이 안전합니다.

## @import: 다른 파일 가져오기

CLAUDE.md가 길어지면 다른 파일로 나누고 가져올 수 있습니다:

```markdown
# 프로젝트 가이드

프로젝트 개요는 @README.md 를 참고하세요.
사용 가능한 명령어는 @package.json 을 확인하세요.

## 추가 지침
- Git 워크플로우: @docs/git-instructions.md
```

`@파일경로` 문법을 쓰면 해당 파일 내용이 Context Window에 함께 로딩됩니다.

**주의사항:**
- 최대 5단계까지 재귀적으로 가져올 수 있음
- 처음 외부 파일을 가져올 때 승인 다이얼로그가 표시됨
- 경로는 CLAUDE.md 파일 위치 기준 상대 경로

## .claude/rules/: 경로별 규칙

특정 파일이나 폴더에만 적용되는 규칙을 만들 수 있습니다:

```
프로젝트/
├── .claude/
│   ├── CLAUDE.md
│   └── rules/
│       ├── code-style.md      # 코드 스타일 규칙
│       ├── testing.md         # 테스트 규칙
│       └── security.md        # 보안 규칙
```

### 경로 조건 설정

rules 파일에 YAML frontmatter로 적용 경로를 지정할 수 있습니다:

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API 개발 규칙

- 모든 API 엔드포인트에 입력 검증 포함
- 표준 에러 응답 형식 사용
```

이 규칙은 `src/api/` 하위의 TypeScript 파일을 작업할 때만 로딩됩니다.

## CLAUDE.md 계층 전체 맵

공식 문서 기준 전체 계층은 다음과 같습니다:

| Scope | 위치 | 공유 | 설명 |
|-------|------|------|------|
| 관리 정책 | `/Library/Application Support/ClaudeCode/CLAUDE.md` (macOS) | 조직 전체 | IT 관리자가 배포 |
| 사용자 전역 | `~/.claude/CLAUDE.md` | 개인 | 모든 프로젝트에 적용 |
| 사용자 전역 규칙 | `~/.claude/rules/*.md` | 개인 | 전역 규칙 파일들 |
| 프로젝트 | `./CLAUDE.md` 또는 `./.claude/CLAUDE.md` | 팀 공유 | 프로젝트 규칙 |
| 프로젝트 규칙 | `./.claude/rules/*.md` | 팀 공유 | 경로별 규칙 |
| 로컬 | `./CLAUDE.local.md` | 개인 | 프로젝트별 개인 설정 |
| 하위폴더 | `./subfolder/CLAUDE.md` | 팀 공유 | 하위폴더에서만 적용 |

## claudeMdExcludes: 불필요한 CLAUDE.md 무시

대규모 모노레포에서 다른 팀의 CLAUDE.md가 로딩되는 것을 방지할 수 있습니다:

```json
{
  "claudeMdExcludes": [
    "**/other-team/CLAUDE.md",
    "**/legacy/.claude/rules/**"
  ]
}
```

`.claude/settings.local.json`에 설정합니다.

## 더 알아보기

- [공식 문서: How Claude remembers your project](https://code.claude.com/docs/en/memory)
