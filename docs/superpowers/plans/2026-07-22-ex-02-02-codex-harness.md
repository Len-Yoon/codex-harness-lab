# 실습 02 Codex 2인 커밋 메시지 하네스 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Codex가 author·reviewer subagent를 순차 호출해 Conventional Commits 메시지 초안과 PASS/REDO 검토 기록을 남기는 실행형 실습을 구성함.

**Architecture:** 실습 폴더의 `AGENTS.md`가 공통 규칙과 안전 경계를 제공함. `.codex/agents/`의 두 custom agent가 `_workspace/` 파일을 통해 인수인계하고, `.agents/skills/commit-message/SKILL.md`가 사전 조건·순차 호출·최대 2회 재작성을 조정함.

**Tech Stack:** Markdown, TOML, Git staged diff, Codex project-scoped custom agents, Codex repository skill discovery.

## Global Constraints

- 원본 다운로드 경로 `/Users/len/Downloads/harness-engineering-with-cc-main/ex-02-02-my-first-harness/`는 읽기 전용 기준 자료로만 사용함.
- Claude Code 전용 `.claude/` 및 `CLAUDE.md`는 복사하지 않으며 Codex의 `.codex/agents/`, `.agents/skills/`, `AGENTS.md`로 변환함.
- 하네스는 `git commit`, `git push`, 파일 삭제를 실행하지 않음.
- author는 초안 파일만, reviewer는 리뷰 파일만 작성함.
- REDO 후 author 재호출은 최대 2회이며 이후 수동 검토 권고로 종료함.
- 사용자 요청 전 커밋·푸시를 수행하지 않음.

---

### Task 1: 실습 골격과 프로젝트 규칙 작성

**Files:**
- Create: `exercises/ex-02-02-my-first-harness/README.md`
- Create: `exercises/ex-02-02-my-first-harness/AGENTS.md`
- Create: `exercises/ex-02-02-my-first-harness/source-reference.md`
- Create: `exercises/ex-02-02-my-first-harness/_workspace/.gitkeep`

**Interfaces:**
- Consumes: 설계서 `docs/superpowers/specs/2026-07-22-ex-02-02-codex-harness-design.md`
- Produces: agent·Skill이 공통으로 참조할 산출물 경로와 실행 절차

- [ ] **Step 1: 실행 전 기준을 문서화함**

`README.md`에 다음 실행 명령을 포함함.

```text
1. 변경 파일을 `git add <파일>`로 스테이징함.
2. 이 폴더를 작업 폴더로 연 Codex에 `커밋 메시지 만들어줘`를 요청함.
3. `_workspace/commit-draft.md`와 `_workspace/review-report.md`를 확인함.
4. 실제 커밋은 사용자가 검토 후 별도로 실행함.
```

- [ ] **Step 2: 하위 작업 규칙을 작성함**

`AGENTS.md`에 Conventional Commits 형식, `_workspace/` 산출물 위치, author·reviewer 쓰기 경계, 실제 커밋·푸시 금지를 명시함.

- [ ] **Step 3: 원본 매핑을 기록함**

`source-reference.md`에 원본 파일 경로와 다음 변환 표를 기록함.

| 원본 | Codex 변환 |
|---|---|
| `CLAUDE.md` | `AGENTS.md` |
| `.claude/agents/*.md` | `.codex/agents/*.toml` |
| `.claude/skills/commit-message/SKILL.md` | `.agents/skills/commit-message/SKILL.md` |

- [ ] **Step 4: 골격 검증을 실행함**

Run:

```bash
test -f exercises/ex-02-02-my-first-harness/README.md && test -f exercises/ex-02-02-my-first-harness/AGENTS.md && test -f exercises/ex-02-02-my-first-harness/source-reference.md && test -f exercises/ex-02-02-my-first-harness/_workspace/.gitkeep
```

Expected: exit code 0.

### Task 2: author·reviewer custom agent 정의

**Files:**
- Create: `exercises/ex-02-02-my-first-harness/.codex/agents/commit-msg-author.toml`
- Create: `exercises/ex-02-02-my-first-harness/.codex/agents/commit-msg-reviewer.toml`

**Interfaces:**
- Consumes: Task 1의 `AGENTS.md`, 스테이지된 diff, `_workspace/` 산출물 경로
- Produces: `commit-draft.md`, `review-report.md`

- [ ] **Step 1: author 역할을 정의함**

author TOML은 `name`, `description`, `developer_instructions`를 정의하고 다음 의무를 포함함.

```toml
name = "commit-msg-author"
description = "스테이지된 Git 변경을 근거로 Conventional Commits 초안을 작성한다."
developer_instructions = """
`git diff --cached`와 `git log -10 --oneline`만 근거로 사용한다.
`_workspace/commit-draft.md`만 작성한다.
실제 커밋·푸시를 실행하지 않는다.
"""
```

- [ ] **Step 2: reviewer 역할을 정의함**

reviewer TOML은 `_workspace/commit-draft.md`와 `git diff --cached`를 대조하고, `_workspace/review-report.md`에 PASS 또는 REDO·사유·수정 지시를 기록하도록 정의함.

- [ ] **Step 3: 역할 경계를 정적 검증함**

Run:

```bash
rg -n '^name =|^description =|^developer_instructions =' exercises/ex-02-02-my-first-harness/.codex/agents/*.toml
```

Expected: 두 파일에서 필수 필드가 각각 한 번씩 확인됨.

### Task 3: 2인 순차 실행 Skill 작성

**Files:**
- Create: `exercises/ex-02-02-my-first-harness/.agents/skills/commit-message/SKILL.md`

**Interfaces:**
- Consumes: Task 2의 `commit-msg-author`, `commit-msg-reviewer`, `_workspace/` 산출물
- Produces: 사용자에게 제시할 최종 초안 또는 수동 검토 권고

- [ ] **Step 1: Skill 메타데이터를 정의함**

```markdown
---
name: commit-message
description: 스테이지된 변경으로 Conventional Commits 커밋 메시지를 author·reviewer subagent 순차 검토로 생성할 때 사용함. `커밋 메시지`, `commit message`, `커밋 메시지 만들어줘` 요청에서 사용함. 실제 커밋·푸시는 수행하지 않음.
---
```

- [ ] **Step 2: 실행 절차를 작성함**

Skill 본문에 다음 순서를 명시함.

```text
1. `git diff --cached --quiet`의 exit code를 확인함.
2. exit code 0이면 `git add` 안내 후 종료함.
3. exit code 1이면 `commit-msg-author`를 호출해 draft를 작성함.
4. `commit-msg-reviewer`를 호출해 PASS/REDO 리포트를 작성함.
5. REDO이면 수정 지시와 함께 author를 재호출함. 최대 2회로 제한함.
6. PASS 또는 한계 도달 시 draft와 review-report 경로를 사용자에게 안내함.
```

- [ ] **Step 3: 안전 조건을 작성함**

Skill에 `git commit`, `git push`, `git reset`, 파일 삭제를 호출하지 않으며 author와 reviewer를 병렬로 실행하지 않는다고 명시함.

- [ ] **Step 4: Skill 형식을 검증함**

Run:

```bash
rg -n '^name: commit-message$|^description:' exercises/ex-02-02-my-first-harness/.agents/skills/commit-message/SKILL.md
```

Expected: name과 description이 확인됨.

### Task 4: 실행 방법과 파일 기반 인수인계 검증

**Files:**
- Modify: `exercises/README.md`
- Modify: `README.md`
- Create: `exercises/ex-02-02-my-first-harness/verification-checklist.md`

**Interfaces:**
- Consumes: Task 1~3의 실행 파일
- Produces: 사용자가 Codex Desktop에서 수행할 수 있는 수동 실행·검증 기준

- [ ] **Step 1: 실습 목록과 시작 방법을 갱신함**

루트 `README.md`에 `ex-02-02-my-first-harness` 실습을 추가하고, `exercises/README.md`가 없으면 새로 만들지 않고 해당 항목은 생략함.

- [ ] **Step 2: 체크리스트를 작성함**

`verification-checklist.md`에 스테이지된 테스트 변경 준비, Skill 호출, draft 생성, reviewer PASS/REDO 기록, 재호출 한계, Git 상태 불변을 체크 항목으로 작성함.

- [ ] **Step 3: 문서 링크와 형식을 검증함**

Run:

```bash
git diff --check
rg -n 'ex-02-02-my-first-harness|commit-msg-author|commit-msg-reviewer|commit-message' README.md exercises/ex-02-02-my-first-harness/README.md exercises/ex-02-02-my-first-harness/verification-checklist.md
```

Expected: 공백 오류가 없고, 실습명·두 agent·Skill 참조가 모두 확인됨.
