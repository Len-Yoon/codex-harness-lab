# 실습 02 — Codex 2인 커밋 메시지 하네스 설계

## 문서 정보

| 항목 | 내용 |
|---|---|
| 문서명 | 실습 02 Codex 2인 커밋 메시지 하네스 설계 |
| 작성일 | 2026-07-22 |
| 기준 자료 | `/Users/len/Downloads/harness-engineering-with-cc-main/ex-02-02-my-first-harness/` |
| 목적 | Claude Code 전용 최소 하네스를 Codex의 프로젝트 로컬 설정으로 변환함 |
| 범위 | 커밋 메시지 초안·검토·PASS/REDO 흐름만 포함하며 실제 커밋·푸시는 제외함 |

## 목표

스테이지된 Git 변경을 대상으로 author subagent와 reviewer subagent가 순차 실행되어 Conventional Commits 형식의 커밋 메시지를 작성·검증하고, 중간 산출물을 `_workspace/`에 남기도록 구성함.

## 구조

```text
exercises/ex-02-02-my-first-harness/
├── README.md
├── AGENTS.md
├── source-reference.md
├── .codex/agents/
│   ├── commit-msg-author.toml
│   └── commit-msg-reviewer.toml
├── .agents/skills/commit-message/SKILL.md
└── _workspace/.gitkeep
```

| 구성 요소 | 책임 | Codex 방식 |
|---|---|---|
| `AGENTS.md` | Conventional Commits, 산출물 위치, 안전 기준을 프로젝트 규칙으로 제공 | 하위 작업 폴더에서 자동 적용되는 지침 파일 |
| author | 스테이지된 변경과 최근 커밋을 근거로 초안 작성 | `.codex/agents/commit-msg-author.toml` custom agent |
| reviewer | 초안 형식·사실 일치 검증 및 PASS/REDO 판정 | `.codex/agents/commit-msg-reviewer.toml` custom agent |
| Skill | 사전 조건 확인, 두 역할 순차 호출, 재작성 횟수 제한 | `.agents/skills/commit-message/SKILL.md` |
| `_workspace/` | 역할 간 인수인계 및 실행 증적 보관 | `commit-draft.md`, `review-report.md` 생성 위치 |

## 실행 흐름

1. 사용자가 `커밋 메시지 만들어줘`와 같은 요청을 전달함.
2. Skill은 `git diff --cached --quiet`로 스테이지된 변경 존재 여부를 확인함.
3. 변경이 없으면 `git add` 필요 사항을 안내하고 종료함.
4. author subagent는 `git diff --cached`, 최근 10개 커밋을 읽고 `_workspace/commit-draft.md`에 초안을 작성함.
5. reviewer subagent는 초안과 `git diff --cached`를 대조하고 `_workspace/review-report.md`에 PASS 또는 REDO와 사유를 작성함.
6. PASS이면 Skill은 초안을 사용자에게 제시하고 종료함.
7. REDO이면 reviewer의 수정 지시를 포함해 author를 재호출함. 재호출은 최대 2회로 제한함.
8. 2회 재호출 후에도 REDO이면 마지막 초안·리포트와 수동 검토 권고를 제시하고 종료함.

## 역할 경계 및 데이터 흐름

| 역할 | 읽기 | 쓰기 | 금지 사항 |
|---|---|---|---|
| author | `git diff --cached`, `git log -10 --oneline`, 이전 `review-report.md`의 REDO 지시 | `_workspace/commit-draft.md` | 실제 커밋·푸시, diff에 없는 사실 추가 |
| reviewer | `git diff --cached`, `_workspace/commit-draft.md` | `_workspace/review-report.md` | 초안 직접 수정, 실제 커밋·푸시 |
| Skill 실행 주체 | 두 산출물과 판정값 | 없음 | author·reviewer 역할 혼합, 무제한 재호출 |

## 산출물 형식

### `commit-draft.md`

```text
type(scope): subject

변경 이유 또는 주요 변경 내용을 3줄 이내로 작성함.
```

### `review-report.md`

```text
# 커밋 메시지 리뷰 보고서

## 판정

PASS | REDO

## 사유

- 형식, 범위, diff 사실 일치 여부를 근거와 함께 기록함.

## 수정 지시

- REDO일 때만 author가 바로 반영할 수 있는 지시를 기록함.
```

## 검증 기준

| 항목 | 확인 방법 | 통과 기준 |
|---|---|---|
| Custom agent 인식 | Codex에서 agent 이름을 지정해 호출 | author·reviewer 역할이 각각 선택 가능함 |
| Skill 인식 | `커밋 메시지 만들어줘` 요청 | `commit-message` Skill이 선택되거나 명시 호출 가능함 |
| 초안 생성 | 스테이지된 테스트 변경으로 실행 | `_workspace/commit-draft.md`가 생성됨 |
| 검토 생성 | author 실행 후 reviewer 실행 | `_workspace/review-report.md`에 PASS 또는 REDO가 기록됨 |
| 재호출 제한 | 형식 오류 초안을 대상으로 검토 | REDO 재작성은 최대 2회 후 종료됨 |
| 안전성 | 실행 전후 Git 상태 확인 | `git commit`, `git push`, 파일 삭제가 발생하지 않음 |

## 범위 제외

- 실제 커밋 실행, 원격 푸시, PR 생성
- 커밋 메시지 외의 코드 검토
- Claude Code 전용 `.claude/` 설정 파일의 복사

## 확인 필요 사항

- Custom agent와 Skill의 발견·호출 UI는 Codex 앱 버전 및 활성 기능에 따라 다를 수 있으므로, 작성 후 현재 Desktop 환경에서 직접 실행 확인 필요함.
