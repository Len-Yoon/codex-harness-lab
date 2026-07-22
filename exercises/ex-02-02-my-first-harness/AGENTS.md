# 커밋 메시지 하네스 작업 규칙

## 공통 규칙

- 커밋 메시지는 `type(scope): subject` 형식의 Conventional Commits를 따름.
- 메시지 내용은 스테이지된 Git 변경과 최근 커밋 이력에서 확인되는 사실만 근거로 작성함.
- 실제 커밋·푸시, Git 이력 변경, 파일 삭제를 수행하지 않음.
- 역할 간 인수인계 산출물은 `_workspace/`에만 보관함.

## 역할별 쓰기 경계

| 역할 | 읽기 범위 | 쓰기 범위 | 금지 사항 |
|---|---|---|---|
| author | 스테이지된 diff, 최근 10개 커밋, reviewer의 REDO 수정 지시 | `_workspace/commit-draft.md` | reviewer 보고서 수정, 실제 커밋·푸시 |
| reviewer | 스테이지된 diff, `_workspace/commit-draft.md` | `_workspace/review-report.md` | author 초안 직접 수정, 실제 커밋·푸시 |

## 산출물 형식

`_workspace/commit-draft.md`에는 Conventional Commits 제목과 변경 이유 또는 주요 변경 내용을 3줄 이내로 기록함.

`_workspace/review-report.md`에는 PASS 또는 REDO 판정, 근거가 있는 사유, REDO 시 author가 반영할 수정 지시를 기록함.

## 검토 흐름

- author와 reviewer는 순차적으로 실행함.
- REDO 판정 시 author 재호출은 최대 2회로 제한함.
- 재작성 한계 도달 후에도 REDO이면 마지막 산출물과 수동 검토 필요 사항을 안내함.
