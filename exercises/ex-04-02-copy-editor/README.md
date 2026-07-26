# 실습 04-02: 제한된 기계 교정 에이전트

## 목적

Codex custom agent가 원고를 직접 수정하되, 정해진 기계적 교정 3종만 수행하도록 제한하는 방법을 실습함.

## 구성

| 경로 | 용도 |
|---|---|
| `.codex/agents/copy-editor.toml` | Codex용 기계 교정 agent 설정 |
| `AGENTS.md` | 수정 범위, 예외, 보고 형식 규칙 |
| `practice-target/ch-07-draft.md` | 교정 대상 가상 원고 |
| `practice-target/README.md` | 원고 사용 규칙 |
| `verification-checklist.md` | 교정 결과 검토표 |
| `result/` | 사용자가 채팅 결과를 직접 보관할 선택 폴더 |
| `source-reference.md` | 원본 예제와 변환 항목 대응표 |

## 실행 방법

1. Codex에서 아래 폴더 전체를 작업 폴더로 열기

   ```text
   /Users/len/Documents/Codex/codex-harness-lab/exercises/ex-04-02-copy-editor
   ```

2. 새 task에서 `copy-editor` agent를 선택하기.

3. 아래 요청문을 그대로 입력하기.

   ```text
   practice-target/ch-07-draft.md를 콜론·Bold 조사·명백한 오탈자 기준으로 교정해 주세요.
   예외 케이스는 수정하지 말고, 수정 내역과 보류 항목은 채팅으로 보고해 주세요.
   ```

4. 변경된 `practice-target/ch-07-draft.md`와 [verification-checklist.md](verification-checklist.md)를 비교해 결과를 확인하기.

5. 보관이 필요하면 채팅 결과를 사용자가 직접 `result/copy-edit-report.md`에 저장하기.

## 교정 범위

| 수정함 | 수정하지 않음 |
|---|---|
| 산문 문장 끝의 불필요한 콜론 | 시간 표기 `12:30` |
| Bold 마크업 밖으로 나온 한글 조사 | 인라인 코드 또는 코드 블록의 콜론 |
| 명백한 중복·오탈자 | 표 헤더·구분 용도의 콜론 |

의미·논리·문체를 다듬는 윤문은 이 실습의 범위가 아님. 고칠지 모호한 항목은 원고를 수정하지 않고 보류로 보고함.

## 주의 사항

- 이 agent는 `practice-target/`의 원고 파일만 수정하도록 제한됨.
- 설정, Git 상태, 원고 밖의 파일, 결과 파일을 변경하면 안 됨.
- 결과 보고서는 자동 저장되지 않음.
