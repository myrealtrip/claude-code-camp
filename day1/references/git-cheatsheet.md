# Git 치트시트

> Day 1 강의에서 배운 3가지 명령어 외에 알아두면 유용한 Git 명령어 모음

## 기본 명령어 (Day 1에서 배운 것)

| 명령어 | 설명 |
|--------|------|
| `git init` | 현재 폴더를 Git으로 관리 시작 |
| `git add .` | 모든 변경사항을 기록 준비 (스테이징) |
| `git commit -m "메시지"` | 변경사항 기록 (커밋) |

## 상태 확인

| 명령어 | 설명 |
|--------|------|
| `git status` | 현재 어떤 파일이 변경되었는지 확인 |
| `git log` | 지금까지의 커밋 이력 보기 |
| `git log --oneline` | 커밋 이력을 한 줄씩 간단히 보기 |
| `git diff` | 아직 커밋하지 않은 변경사항 보기 |

## 되돌리기

| 명령어 | 설명 |
|--------|------|
| `git checkout -- 파일명` | 특정 파일의 변경사항 되돌리기 |
| `git reset HEAD 파일명` | 스테이징 취소 (add 취소) |

## 자주 쓰는 패턴

### 작업 → 기록의 반복

```bash
# 1. 작업하기 (파일 수정, 생성 등)
# 2. 변경사항 확인
git status

# 3. 기록할 파일 추가
git add .

# 4. 커밋
git commit -m "무엇을 했는지 설명"
```

### 커밋 메시지 잘 쓰는 법

```bash
# 좋은 예
git commit -m "로그인 페이지 디자인 수정"
git commit -m "보고서 템플릿 추가"
git commit -m "오타 수정"

# 나쁜 예
git commit -m "수정"
git commit -m "작업"
git commit -m "asdf"
```

커밋 메시지는 **나중에 돌아봤을 때 무슨 작업이었는지 알 수 있게** 적는 것이 좋습니다.

## Claude Code와 Git

Claude Code는 Git을 적극 활용합니다:

- Claude가 파일을 수정하면 `git diff`로 뭐가 바뀌었는지 확인 가능
- 마음에 안 들면 `git checkout -- 파일명`으로 되돌리기 가능
- Claude에게 "커밋해줘"라고 말하면 알아서 커밋 메시지를 만들고 커밋해줌

## 더 알아보기

- [Git 공식 문서](https://git-scm.com/doc)
- [GitHub에서 제공하는 Git 가이드](https://docs.github.com/en/get-started/getting-started-with-git)
