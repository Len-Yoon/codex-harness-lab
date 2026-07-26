# 실습 04-01: 읽기 전용 보안 분석 에이전트

## 목적

Claude 원본의 `sample-app/` 의도적 취약 Flask 예제를 대상으로, Codex custom agent가 코드를 수정하지 않고 정적 보안 분석 결과를 채팅으로 보고하도록 실습함.

## 구성

| 경로 | 용도 |
|---|---|
| `.codex/agents/security-analyst.toml` | Codex용 읽기 전용 보안 분석 agent 설정 |
| `AGENTS.md` | 분석 범위와 보고 형식 규칙 |
| `sample-app/app.py` | Flask 기반 의도적 취약 연습 대상 |
| `sample-app/db.py` | `app.py`가 사용하는 DB 헬퍼 |
| `sample-app/README.md` | 대상 코드의 경고와 점검 항목 |
| `verification-checklist.md` | 실습 결과 검토 항목 |
| `result/` | 사용자가 채팅 결과를 직접 보관할 선택 폴더 |

## 실행 방법

1. Codex에서 이 폴더 전체를 작업 폴더로 열기

   ```text
   /Users/len/Documents/Codex/codex-harness-lab/exercises/ex-04-01-security-analyst
   ```

2. 새 task에서 `security-analyst` agent를 선택하기. 선택 목록이 보이지 않으면 이 폴더를 작업 폴더로 다시 열고 새 task를 시작하기.

3. 아래 요청문을 입력하기.

   ```text
   sample-app/ 디렉토리의 보안 취약점을 분석해 주세요.
   Critical/High/Medium/Low, 파일:라인, 설명, 권장 수정안,
   가능한 경우 OWASP/CWE를 포함하세요. 코드는 수정하지 마세요.
   ```

4. 채팅으로 받은 분석 결과를 `verification-checklist.md`로 확인하기. 보관이 필요하면 사용자가 직접 `result/security-report.md`로 저장하기.

## 주의 사항

- `sample-app/`은 취약점 식별 학습용 정적 예시임. 실행하거나 배포하면 안 됨.
- agent는 읽기 전용으로 설정됨. 코드·설정·Git 상태를 변경하지 않아야 함.
- `result/security-report.md`는 자동 생성되지 않음. 채팅 결과를 사용자가 검토한 뒤 필요할 때만 직접 보관함.

## 다른 프로젝트에 적용하기

대상 프로젝트의 루트에 `.codex/agents/security-analyst.toml`을 배치한 후, 해당 프로젝트를 Codex 작업 폴더로 열어 같은 방식으로 실행함. 대상 코드와 설정은 사전에 별도로 백업·검토 필요.
