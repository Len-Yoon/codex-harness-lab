# 실습 01 — 하네스 A/B 비교: Codex 사용 팁 정리

## 실습 개요

동일한 요청을 하네스 없이 한 번, `AGENTS.md`가 있는 작업 공간에서 한 번 수행하여 결과 차이를 확인함.

| 구분 | A. 하네스 미적용 | B. 하네스 적용 |
|---|---|---|
| 작업 위치 | `non-harness/` | `apply-harness/` |
| 사전 규칙 | 없음 | `apply-harness/AGENTS.md` |
| 요청문 | `prompts/same-request.md` 내용 그대로 사용 | 동일 |
| 결과 형식 | 자유 형식 | 팁별 표준 형식 및 인덱스 |
| 비교 자료 | `non-harness/codex-tips.md` | `apply-harness/tips/` |

## 실행 절차

### 1. A 실험 — 하네스 미적용

1. Codex에서 `non-harness/`를 기준 작업 폴더로 열거나, 결과를 이 폴더에 저장하도록 요청함.
2. [`prompts/same-request.md`](prompts/same-request.md)의 요청문만 그대로 입력함.
3. 결과를 `non-harness/codex-tips.md`에 붙여 넣음.
4. 이 단계에서는 `apply-harness/AGENTS.md`를 참고하지 않음.

### 2. B 실험 — 하네스 적용

1. Codex에서 `apply-harness/`를 기준 작업 폴더로 열거나, 이 폴더의 `AGENTS.md`를 적용하도록 요청함.
2. A 실험과 동일하게 [`prompts/same-request.md`](prompts/same-request.md)의 요청문만 입력함.
3. 리서치 근거를 `apply-harness/research-notes.md`에 기록함.
4. 결과는 주제별 파일로 `apply-harness/tips/`에 저장하고 `tips/README.md`를 갱신함.

### 3. 결과 비교

1. `evaluation.md`의 A·B 열에 실제 수치와 근거를 입력함.
2. 각 점수의 근거 파일 또는 섹션을 비고에 연결함.
3. 점수만으로 결론 내리지 말고, 하네스가 유효했던 부분과 불필요했던 부분을 각각 기록함.

## 유의 사항

- 요청문 외의 추가 요구사항을 A 또는 B 중 한쪽에만 제공하면 비교가 무효가 됨.
- B의 출처는 공식 OpenAI 문서 또는 직접 실행 기록만 사용함.
- 네트워크나 권한 문제로 검증하지 못한 기능은 팁에 넣지 않거나 `확인 필요`로 표시함.
