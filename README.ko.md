# Sub-agent Wizard

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE.txt)

> 대화형 질문을 통해 Claude Code 서브에이전트를 생성하는 위자드

[English](README.md)

## 개요

Sub-agent Wizard는 5단계 대화형 워크플로우를 통해 효과적인 서브에이전트를 생성하도록 안내하는 Claude Code 스킬입니다. 문서를 읽고 구조를 직접 파악하는 대신, 위자드가 핵심 질문을 던지고 최적화된 서브에이전트 파일을 생성합니다.

## 기능

- **5단계 가이드 워크플로우**: 목적 발견부터 서브에이전트 생성까지 단계별 진행
- **Description 최적화**: 자동 위임을 위한 효과적인 트리거 설명 작성 가이드
- **도구 선택 전략**: 최소 권한 원칙에 따른 일반적인 도구 조합
- **유형별 템플릿**: 8가지 서브에이전트 유형에 대한 즉시 사용 가능한 템플릿
- **검증 체크리스트**: 일반적인 실수를 방지하는 사전 생성 검증

## 서브에이전트란?

서브에이전트는 Claude Code가 특정 작업을 처리하기 위해 생성하는 전문화된 AI 어시스턴트입니다:

- **커스텀 시스템 프롬프트** - 특정 도메인에 맞춤화된 지침
- **도구 제한** - 파일 및 시스템 도구에 대한 제어된 접근
- **모델 선택** - 작업 복잡도에 따라 haiku, sonnet, opus 선택
- **자동 위임** - Claude가 매칭되는 작업을 사전에 위임

## 설치

### Claude Code 스킬로 설치

```bash
# Claude Code skills 디렉토리에 클론
git clone https://github.com/greeun/subagent-wizard.git ~/.claude/skills/subagent-wizard
```

### 또는 로컬 개발에서 심링크

```bash
ln -s /path/to/subagent-wizard ~/.claude/skills/subagent-wizard
```

### 설치 확인

Claude Code를 재시작하면 서브에이전트 생성을 언급할 때 스킬이 자동으로 활성화됩니다.

## 사용법

### 자동 활성화

Claude에게 서브에이전트 생성을 요청하세요:

```
"코드 리뷰용 서브에이전트 만들어줘"
"디버깅 전문 에이전트 만들고 싶어"
"테스트 실행하는 서브에이전트 생성해줘"
"I want to create a sub-agent for code review"
```

### 위자드 워크플로우

위자드가 5단계를 안내합니다:

```
Phase 1: 목적 발견 (Purpose Discovery)
    ↓
Phase 2: 구성 설계 (Configuration Design)
    ↓
Phase 3: 시스템 프롬프트 작성 (System Prompt Writing)
    ↓
Phase 4: 범위 선택 (Scope Selection)
    ↓
Phase 5: 검증 및 생성 (Validation & Creation)
```

#### Phase 1: 목적 발견

서브에이전트가 해야 할 일 파악:
- 어떤 작업이나 도메인을 처리해야 하나요?
- 트리거되어야 하는 요청 예시
- 자동 vs 명시적 위임
- 성공 기준

#### Phase 2: 구성 설계

기술적 구성 정의:
- **Name**: `소문자-하이픈` 형식
- **Description**: 기능 + 트리거 시나리오 + proactive 키워드
- **Tools**: 읽기 전용, 수정, 또는 전체 접근
- **Model**: haiku (빠름), sonnet (균형), opus (복잡)
- **Permission mode**: default, acceptEdits, bypassPermissions, plan

#### Phase 3: 시스템 프롬프트 작성

효과적인 시스템 프롬프트 작성:
- 역할 정의
- 호출 시 초기 행동
- 책임과 제약
- 출력 형식

#### Phase 4: 범위 선택

저장 위치 선택:
- `.claude/agents/` - 프로젝트 전용 (높은 우선순위)
- `~/.claude/agents/` - 사용자 전체 (모든 프로젝트)

#### Phase 5: 검증 및 생성

최종 확인 및 파일 생성:
- 사전 생성 체크리스트
- 테스트 시나리오 생성
- 파일 작성 및 검증

## 서브에이전트 유형

| 유형 | 용도 | 도구 |
|------|------|------|
| **Code Reviewer** | 품질 및 보안 리뷰 | Read, Grep, Glob, Bash |
| **Debugger** | 오류 분석 및 수정 | Read, Edit, Bash, Grep, Glob |
| **Test Runner** | 테스트 실행 및 수정 | Bash, Read, Edit, Grep, Glob |
| **Data Analyst** | SQL 및 데이터 인사이트 | Bash, Read, Write |
| **Doc Writer** | 문서 작성 | Read, Write, Edit, Glob |
| **Security Auditor** | 취약점 스캔 | Read, Grep, Glob |
| **API Developer** | REST/GraphQL 개발 | Read, Write, Edit, Grep, Glob, Bash |
| **DevOps Engineer** | CI/CD 및 인프라 | Read, Write, Edit, Bash, Grep, Glob |

## 프로젝트 구조

```
subagent-wizard/
├── SKILL.md                          # 메인 위자드 가이드
├── LICENSE.txt                       # Apache 2.0 라이선스
├── README.md                         # 영어 문서
├── README.ko.md                      # 한국어 문서 (이 파일)
├── references/
│   ├── tool-combinations.md          # 도구 선택 가이드
│   ├── type-templates.md             # 서브에이전트 유형별 템플릿
│   └── description-examples.md       # Description 작성 예시
└── assets/
    └── subagent-template.md          # 기본 서브에이전트 템플릿
```

## 빠른 시작 예제

3단계로 코드 리뷰어 서브에이전트 생성:

### 1. 디렉토리 생성

```bash
mkdir -p ~/.claude/agents
```

### 2. 서브에이전트 파일 생성

`~/.claude/agents/code-reviewer.md` 작성:

```markdown
---
name: code-reviewer
description: 코드 리뷰 전문가. 코드 수정 후 PROACTIVELY 사용.
tools: Read, Grep, Glob, Bash
model: inherit
---

당신은 시니어 코드 리뷰어입니다.

호출 시:

1. git diff로 변경사항 확인
2. 수정된 파일 검토
3. 우선순위별로 이슈 보고

집중 영역:
- 코드 가독성
- 보안 취약점
- 에러 처리
- 모범 사례
```

### 3. 테스트

Claude에게: "최근 코드 변경사항 리뷰해줘"

## 구성 참조

### Frontmatter 필드

| 필드 | 필수 | 값 | 설명 |
|------|------|-----|------|
| `name` | 예 | `소문자-하이픈` | 서브에이전트 식별자 |
| `description` | 예 | 문자열 | 사용 시점 (자동 위임에 중요) |
| `tools` | 아니오 | 쉼표 구분 | 도구 접근 (생략 시 전체 접근) |
| `model` | 아니오 | `inherit`, `haiku`, `sonnet`, `opus` | AI 모델 선택 |
| `permissionMode` | 아니오 | `default`, `acceptEdits`, `bypassPermissions`, `plan` | 권한 처리 |
| `skills` | 아니오 | 쉼표 구분 | 자동 로드 스킬 |

### 도구 카테고리

| 카테고리 | 도구 | 사용 사례 |
|----------|------|----------|
| 읽기 전용 | `Read, Grep, Glob, Bash` | 분석, 감사 |
| 수정 | `Read, Write, Edit, Grep, Glob, Bash` | 버그 수정, 기능 개발 |
| 최소 | `Read, Grep, Glob` | 보안 리뷰 |
| 전체 | (필드 생략) | 범용 |

### 자동 위임 키워드

자동 작업 위임을 위해 description에 포함:

- `Use PROACTIVELY when...`
- `MUST BE USED when...`
- `Use immediately after...`

## subagent-creator와의 통합

Sub-agent Wizard는 기존 `subagent-creator` 스킬을 보완합니다:

| 기능 | subagent-creator | subagent-wizard |
|------|------------------|-----------------|
| 접근 방식 | 문서 기반 | 질문 기반 |
| Description 도움 | 기본 형식 | 종합 예시 |
| 템플릿 | 단일 템플릿 | 8가지 유형별 템플릿 |
| 도구 선택 | 수동 선택 | 가이드 추천 |
| 대상 사용자 | 숙련자 | 초보자~전문가 |

함께 사용:
1. **subagent-wizard** → 대화형으로 서브에이전트 계획 및 설계
2. **subagent-creator** → 고급 구성 참조

## 일반적인 실수

| 실수 | 문제 | 해결책 |
|------|------|--------|
| 모호한 description | 트리거 안됨 | 구체적인 시나리오와 키워드 추가 |
| 너무 많은 도구 | 집중력 저하 | 필요한 최소 도구만 부여 |
| 일반적인 프롬프트 | 차별화 안됨 | 도메인별 지침 추가 |
| "proactively" 누락 | 자동 위임 안됨 | "Use PROACTIVELY when..." 추가 |
| 잘못된 위치 | 찾을 수 없음 | `.claude/agents/` vs `~/.claude/agents/` 확인 |

## 요구사항

- Claude Code CLI
- 추가 의존성 없음

## 기여

기여를 환영합니다! Pull Request를 자유롭게 제출해주세요.

## 라이선스

Apache License 2.0 - 자세한 내용은 [LICENSE.txt](LICENSE.txt) 참조

## 관련 링크

- [subagent-creator](https://github.com/greeun/subagent-creator) - 서브에이전트 생성 참조
- [skill-wizard](https://github.com/greeun/skill-wizard) - 스킬 생성 위자드
- [Claude Code 문서](https://docs.anthropic.com/claude-code)
