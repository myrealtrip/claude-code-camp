# LLM 기초 개념

> CLAUDE.md와 Memory가 왜 작동하는지 이해하기 위한 배경 지식

## LLM이란?

LLM(Large Language Model)은 대규모 텍스트 데이터를 학습한 AI 모델입니다. Claude가 바로 LLM입니다.

쉽게 말하면: **엄청나게 많은 글을 읽고 학습한 AI**가 여러분의 질문에 답하는 것입니다.

## Context Window: 대화의 기억 한계

### 개념

LLM에는 **Context Window**라는 것이 있습니다. 한 번의 대화에서 기억할 수 있는 양의 한계입니다.

> 비유: 책상 위에 올려놓을 수 있는 종이의 양. 책상이 꽉 차면 오래된 종이부터 치워야 새 종이를 올릴 수 있습니다.

### 왜 중요한가?

- 대화가 길어지면 초반 내용을 잊을 수 있습니다
- CLAUDE.md는 매 대화 시작 시 Context Window에 자동으로 올라갑니다
- 그래서 CLAUDE.md의 지시사항은 항상 기억됩니다

### Context Window와 CLAUDE.md의 관계

```
┌─────────────────────────────────────┐
│         Context Window              │
│                                     │
│  ┌─────────────────────────────┐    │
│  │ CLAUDE.md (항상 로딩됨)       │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ Memory (첫 200줄 로딩됨)      │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ 대화 내용                     │    │
│  │ ...                          │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

CLAUDE.md가 너무 길면 대화에 쓸 수 있는 공간이 줄어듭니다. 그래서 **간결하게 쓰는 것**이 중요합니다.

## System Prompt: 매번 읽는 첫 페이지

### 개념

System Prompt는 AI가 대화를 시작하기 전에 먼저 읽는 지시사항입니다.

> 비유: 연극 배우가 무대에 올라가기 전에 읽는 대본의 첫 페이지. "당신은 이런 역할입니다."

### CLAUDE.md가 System Prompt처럼 작동하는 원리

Claude Code에서 CLAUDE.md는 사실상 System Prompt와 같은 역할을 합니다:

1. 대화가 시작되면 Claude는 먼저 CLAUDE.md를 읽습니다
2. CLAUDE.md의 지시사항을 "이렇게 행동해야겠구나"라고 이해합니다
3. 그 다음 여러분의 질문에 답합니다

그래서 CLAUDE.md에 적는 내용이 Claude의 행동에 직접적인 영향을 줍니다.

## 요약

| 개념 | 비유 | CLAUDE.md와의 관계 |
|------|------|-------------------|
| Context Window | 책상 위 공간 | CLAUDE.md는 항상 책상 위에 올라감 |
| System Prompt | 대본의 첫 페이지 | CLAUDE.md가 이 역할을 함 |
| Token | 글자 수 세기 | CLAUDE.md가 길면 대화 공간이 줄어듦 |

## 더 알아보기

- [Anthropic - Claude 소개](https://www.anthropic.com/claude)
- [Claude Code 공식 문서](https://code.claude.com/docs/en/overview)
