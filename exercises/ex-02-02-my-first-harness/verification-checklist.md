# 실습 02 실행 및 인수인계 검증 체크리스트

## 문서 정보

| 항목 | 내용 |
|---|---|
| 문서명 | 실습 02 실행 및 인수인계 검증 체크리스트 |
| 기준 자료 | `README.md`, `AGENTS.md`, `.codex/agents/`, `.agents/skills/commit-message/SKILL.md` |
| 목적 | Codex Desktop에서 2인 커밋 메시지 하네스의 파일 기반 인수인계와 안전 경계를 수동 검증함 |
| 비고 | 이 체크리스트는 실제 커밋·푸시를 수행하지 않음 |

## 1. 실행 전 준비

- [x] 테스트용 변경으로 `README.md`의 `테스트용 예시 추가` 섹션을 준비함.
- [x] 테스트용 변경 파일 `README.md`만 스테이징함.
- [x] `git diff --cached`로 스테이지된 변경이 테스트용 예시 섹션 추가임을 확인함.
- [x] 이 실습 폴더 `exercises/ex-02-02-my-first-harness/`를 작업 폴더로 Codex에서 열어 실행함.
- [x] 실행 전 산출물 파일이 없음을 확인하고, 실행 후 생성된 파일을 이번 실행 증적으로 구분함.

## 2. Skill 호출 및 순차 실행

- [x] Codex에 `커밋 메시지 만들어줘`를 요청하여 `commit-message` Skill을 호출함.
- [x] 스테이지된 변경이 없다는 안내를 확인한 뒤 `README.md`를 스테이징하고 다시 요청함.
- [x] `commit-msg-author`가 먼저 실행되어 `_workspace/commit-draft.md`를 작성함.
- [x] `commit-msg-reviewer`가 author 완료 후 실행되어 `_workspace/review-report.md`를 작성함.
- [x] author·reviewer 산출물 생성 시각이 각각 22:11:42, 22:12:08로 확인되어 순차 실행을 확인함.

## 3. 산출물 및 판정 확인

| 확인 항목 | 확인 기준 | 결과 | 비고 |
|---|---|---|---|
| 초안 파일 | `_workspace/commit-draft.md`가 생성 또는 갱신됨 | 일치 | `docs(readme): add test example section` 초안 생성 확인됨 |
| 초안 내용 | Conventional Commits `type(scope): subject` 형식이며 스테이지된 diff 사실과 일치함 | 일치 | `README.md`의 테스트용 예시 섹션 추가와 일치함 |
| 리뷰 파일 | `_workspace/review-report.md`가 생성 또는 갱신됨 | 일치 | reviewer 보고서 생성 확인됨 |
| 판정 기록 | 리뷰 파일의 `## 판정`에 `PASS` 또는 `REDO`가 기록됨 | 일치 | `PASS` 기록 확인됨 |
| REDO 사유 | `REDO`인 경우 `## 사유`와 `## 수정 지시`가 기록됨 | 해당 없음 | 최초 검토에서 PASS 판정됨 |

## 4. REDO 재시도 한계 확인

- [x] 최초 검토가 PASS이므로 REDO 재실행은 해당 없음.
- [x] 최초 검토가 PASS이므로 author 재호출 및 reviewer 재검토는 해당 없음.
- [x] REDO 재호출 횟수는 0회로 재작성 한계 이내임.
- [x] 최초 검토가 PASS이므로 REDO 한계 도달 시나리오는 해당 없음.
- [x] PASS 후 최종 초안과 리뷰 보고서 경로가 안내됨.

## 5. Git 상태 및 안전 경계 확인

- [x] 하네스 실행 단계에서 `README.md` 테스트 변경 외 추가 스테이징 변경이 없음을 확인함.
- [x] 하네스 실행 단계에서는 새 커밋을 생성하지 않음. 이후 커밋·병합은 사용자 명시 요청에 따른 별도 Git 작업으로 수행됨.
- [x] 원격 push를 수행하지 않음.
- [x] 하네스 실행 단계에서 `git reset` 또는 파일 삭제를 실행하지 않음.
- [x] author는 `_workspace/commit-draft.md`만, reviewer는 `_workspace/review-report.md`만 작성한 것으로 실행 결과를 확인함.

## 확인 필요 사항

- [ ] Codex Desktop의 custom agent 및 Skill 발견·호출 UI는 현재 설치 버전과 활성 기능에 따라 다를 수 있으므로 화면 기준 확인 필요함.
- [x] 테스트용 변경은 사용자 요청에 따라 실제 커밋 후 `main`에 병합함.

## 후속 조치

- [x] 초안과 리뷰 보고서를 사람이 검토한 뒤, 사용자 요청에 따라 실제 커밋 및 `main` 병합을 수행함.
- [x] 실행 증적 보관을 위해 초안·리뷰 보고서와 갱신된 체크리스트를 함께 커밋 대상으로 지정함.
