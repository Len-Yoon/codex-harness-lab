---
name: commit-message
description: 스테이지된 변경으로 Conventional Commits 커밋 메시지를 author·reviewer subagent 순차 검토로 생성할 때 사용함. `커밋 메시지`, `commit message`, `커밋 메시지 만들어줘` 요청에서 사용함. 실제 커밋·푸시는 수행하지 않음.
---

# 커밋 메시지 2인 검토 절차

## 사전 조건

1. `git diff --cached --quiet`를 실행하고 종료 코드를 확인함.
2. 종료 코드가 `0`이면 스테이지된 변경이 없음을 의미함. 사용자에게 `git add <파일>`로 변경을 스테이징하도록 안내하고 종료함.
3. 종료 코드가 `1`이면 스테이지된 변경이 있음을 의미함. 아래 절차를 진행함.
4. 종료 코드가 `0` 또는 `1` 이외이면 Git 명령 오류로 처리하고 오류 내용을 사용자에게 안내한 뒤 종료함.

## 순차 실행

1. `commit-msg-author` custom agent를 먼저 단독으로 호출함. author는 `_workspace/commit-draft.md`만 작성함.
2. author 완료 후 `commit-msg-reviewer` custom agent를 단독으로 호출함. reviewer는 `_workspace/review-report.md`만 작성함.
3. `_workspace/review-report.md`의 `## 판정`에서 `PASS` 또는 `REDO`를 확인함.
4. `PASS`이면 `_workspace/commit-draft.md`의 최종 초안과 `_workspace/review-report.md` 경로를 사용자에게 제시하고 종료함.
5. `REDO`이면 reviewer 보고서의 `## 수정 지시`를 author에게 전달하여 `commit-msg-author`를 다시 단독으로 호출함. 이후 reviewer를 다시 단독으로 호출함.
6. REDO에 따른 author 재호출은 최초 작성 뒤 최대 2회까지만 허용함. 각 재호출 뒤에는 reviewer를 반드시 다시 호출함.
7. 두 번째 재호출 뒤에도 `REDO`이면 마지막 `_workspace/commit-draft.md`와 `_workspace/review-report.md` 경로를 제시하고 수동 검토를 권고한 뒤 종료함.

author와 reviewer를 병렬로 실행하지 않음. reviewer가 PASS 또는 REDO 판정을 기록하기 전에 다음 단계를 진행하지 않음.

## 안전 경계

- `git commit`, `git push`, `git reset`, 파일 삭제 명령을 호출하지 않음.
- 실제 커밋·푸시·Git 이력 변경을 수행하지 않음.
- author의 쓰기 범위는 `_workspace/commit-draft.md`로 한정하고, reviewer의 쓰기 범위는 `_workspace/review-report.md`로 한정함.
- 사용자 요청 전 스테이지 상태를 변경하지 않음.
