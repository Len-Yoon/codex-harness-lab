# Codex Harness Lab

Codex 하네스 엔지니어링을 책의 예제 단위로 재구성하여 실습하는 저장소임.

## 목적

- 동일한 요청에서 규칙·작업 절차·검증 기준이 산출물에 미치는 영향을 확인함.
- Claude Code 전용 구성은 Codex의 `AGENTS.md`, Skill, 프로젝트 설정으로 치환함.
- 각 실습의 입력·결과·평가 근거를 함께 버전 관리함.

## 실습 목록

| 원본 예제 | Codex 실습 | 상태 |
|---|---|---|
| `ex-02-01-ab-comparison` | 동일 요청 A/B 비교 | 준비 완료 |
| `ex-02-02-my-first-harness` | 2인 커밋 메시지 하네스 | 준비 완료 |
| `ex-04-01-security-analyst` | 읽기 전용 보안 분석 Codex 실습 환경 | 준비 완료 |
| `ex-04-02-copy-editor` | 제한된 기계 교정 Codex 실습 환경 | 준비 완료 |

## 시작 방법

1. `exercises/ex-02-01-ab-comparison/README.md`를 열어 실습 절차를 확인함.
2. A 실험과 B 실험에 `prompts/same-request.md`의 문장을 그대로 사용함.
3. 두 결과를 `evaluation.md`에 기록하고 비교함.
4. 커밋 메시지 하네스는 `exercises/ex-02-02-my-first-harness/README.md`와 `verification-checklist.md`를 확인한 뒤, 해당 폴더를 작업 폴더로 연 Codex에서 `커밋 메시지 만들어줘`를 요청함.
5. 보안 분석 실습은 Codex custom agent를 사용함. `exercises/ex-04-01-security-analyst/` 폴더를 작업 폴더로 열면 `.codex/agents/security-analyst.toml`이 자동으로 인식됨.
6. 기계 교정 실습은 `exercises/ex-04-02-copy-editor/` 폴더를 작업 폴더로 열고 `copy-editor` agent를 선택한 뒤, 대상 원고만 교정하도록 요청함.

## 예제 추가 규칙

- 원본 예제명과 동일한 폴더명을 `exercises/` 아래에 사용함.
- 원본 자료는 `source-reference`로만 기록하고 복사·수정하지 않음.
- Claude Code 전용 기능은 Codex에서 실제 지원되는 방식으로만 변환함.
- 불확실하거나 검증하지 못한 내용은 `확인 필요`로 표시함.
