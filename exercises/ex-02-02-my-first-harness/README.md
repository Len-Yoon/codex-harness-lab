# 실습 02 — Codex 2인 커밋 메시지 하네스

## 목적

스테이지된 Git 변경을 근거로 author와 reviewer가 순차적으로 Conventional Commits 커밋 메시지 초안을 작성·검토하는 하네스임. 이 실습은 메시지 초안과 검토 기록만 생성하며 실제 커밋·푸시는 수행하지 않음.

## 실행 전 기준

1. 변경 파일을 `git add <파일>`로 스테이징함.
2. 이 폴더를 작업 폴더로 연 Codex에 `커밋 메시지 만들어줘`를 요청함.
3. `_workspace/commit-draft.md`와 `_workspace/review-report.md`를 확인함.
4. 실제 커밋은 사용자가 검토 후 별도로 실행함.

## 산출물

| 파일 | 작성 역할 | 내용 |
|---|---|---|
| `_workspace/commit-draft.md` | author | Conventional Commits 형식의 커밋 메시지 초안 |
| `_workspace/review-report.md` | reviewer | PASS 또는 REDO 판정, 사유, 수정 지시 |

## 구성

| 구성 요소 | 역할 |
|---|---|
| `AGENTS.md` | 공통 규칙, 산출물 경로, 역할별 쓰기 경계 제공 |
| `.codex/agents/` | author·reviewer custom agent 정의 위치 |
| `.agents/skills/commit-message/` | 순차 실행과 재작성 제한을 조정하는 Skill 위치 |
| `_workspace/` | 역할 간 인수인계 및 실행 증적 보관 위치 |

## 확인 필요 사항

Custom agent와 Skill의 발견·호출 UI는 Codex Desktop 버전 및 활성 기능에 따라 달라질 수 있으므로 현재 환경에서 직접 실행 확인 필요함.

## 테스트용 예시 추가
