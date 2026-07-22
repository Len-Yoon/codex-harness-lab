# 실습 02 실행 및 인수인계 검증 체크리스트

## 문서 정보

| 항목 | 내용 |
|---|---|
| 문서명 | 실습 02 실행 및 인수인계 검증 체크리스트 |
| 기준 자료 | `README.md`, `AGENTS.md`, `.codex/agents/`, `.agents/skills/commit-message/SKILL.md` |
| 목적 | Codex Desktop에서 2인 커밋 메시지 하네스의 파일 기반 인수인계와 안전 경계를 수동 검증함 |
| 비고 | 이 체크리스트는 실제 커밋·푸시를 수행하지 않음 |

## 1. 실행 전 준비

- [ ] 실제 커밋할 변경과 분리 가능한 테스트용 변경 파일을 준비함.
- [ ] 테스트용 변경 파일만 `git add <파일>`로 스테이징함.
- [ ] `git diff --cached`로 스테이지된 변경 내용이 의도한 테스트 변경인지 확인함.
- [ ] 이 실습 폴더 `exercises/ex-02-02-my-first-harness/`를 작업 폴더로 Codex Desktop에서 열음.
- [ ] 기존 `_workspace/commit-draft.md` 또는 `_workspace/review-report.md`가 있으면 이전 실행 증적인지 구분하여 기록함. 삭제하지 않음.

## 2. Skill 호출 및 순차 실행

- [ ] Codex에 `커밋 메시지 만들어줘`를 요청하여 `commit-message` Skill을 호출함.
- [ ] 스테이지된 변경이 없다는 안내가 나오면 `git add <파일>`로 테스트 변경을 스테이징한 뒤 다시 요청함.
- [ ] `commit-msg-author`가 먼저 실행되어 `_workspace/commit-draft.md`를 작성했는지 확인함.
- [ ] `commit-msg-reviewer`가 author 완료 후 실행되어 `_workspace/review-report.md`를 작성했는지 확인함.
- [ ] author와 reviewer가 병렬 실행되지 않았는지 실행 기록 또는 산출물 생성 순서로 확인함.

## 3. 산출물 및 판정 확인

| 확인 항목 | 확인 기준 | 결과 | 비고 |
|---|---|---|---|
| 초안 파일 | `_workspace/commit-draft.md`가 생성 또는 갱신됨 | ☐ 일치 / ☐ 확인 필요 | |
| 초안 내용 | Conventional Commits `type(scope): subject` 형식이며 스테이지된 diff 사실과 일치함 | ☐ 일치 / ☐ 확인 필요 | |
| 리뷰 파일 | `_workspace/review-report.md`가 생성 또는 갱신됨 | ☐ 일치 / ☐ 확인 필요 | |
| 판정 기록 | 리뷰 파일의 `## 판정`에 `PASS` 또는 `REDO`가 기록됨 | ☐ 일치 / ☐ 확인 필요 | |
| REDO 사유 | `REDO`인 경우 `## 사유`와 `## 수정 지시`가 기록됨 | ☐ 일치 / ☐ 해당 없음 / ☐ 확인 필요 | |

## 4. REDO 재시도 한계 확인

- [ ] `REDO`이면 reviewer의 수정 지시를 반영하도록 author가 다시 실행되었는지 확인함.
- [ ] author 재호출 뒤 reviewer가 다시 실행되어 최신 판정을 기록했는지 확인함.
- [ ] REDO에 따른 author 재호출 횟수가 최초 작성 이후 최대 2회인지 확인함.
- [ ] 두 번째 재호출 뒤에도 `REDO`이면 마지막 초안·리뷰 보고서 경로가 안내되고 수동 검토가 권고되었는지 확인함.
- [ ] `PASS`이면 최종 초안과 리뷰 보고서 경로가 안내되었는지 확인함.

## 5. Git 상태 및 안전 경계 확인

- [ ] 실행 전후 `git status --short` 결과를 비교하여 테스트용 스테이징 변경 외 추가 Git 변경이 없는지 확인함.
- [ ] `git log -1 --oneline`을 실행 전후 비교하여 새 커밋이 생성되지 않았는지 확인함.
- [ ] 원격 저장소에 push되지 않았는지 확인함.
- [ ] `git reset` 또는 파일 삭제가 실행되지 않았는지 실행 기록과 작업 폴더 상태로 확인함.
- [ ] author가 `_workspace/commit-draft.md`만, reviewer가 `_workspace/review-report.md`만 작성 또는 갱신했는지 확인함.

## 확인 필요 사항

- [ ] Codex Desktop의 custom agent 및 Skill 발견·호출 UI는 현재 설치 버전과 활성 기능에 따라 다를 수 있으므로 실제 환경에서 확인 필요함.
- [ ] 테스트가 끝난 뒤 테스트용 스테이징 변경의 처리 방법은 사용자의 Git 작업 정책에 따라 별도 결정 필요함.

## 후속 조치

- [ ] 모든 항목이 충족되면 초안과 리뷰 보고서를 사람이 검토한 후 실제 커밋 여부를 별도 결정함.
- [ ] 하나라도 충족되지 않으면 확인 결과와 산출물 경로를 기록하고 하네스 설정 또는 Codex Desktop 환경을 추가 점검함.
