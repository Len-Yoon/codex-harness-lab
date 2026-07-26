# 원본 예제 대응표

## 기준 자료

| 항목 | 내용 |
|---|---|
| 원본 경로 | `/Users/len/Downloads/harness-engineering-with-cc-main/ex-04-01-security-analyst/` |
| 변환 목적 | Claude Code 전용 agent 설정을 Codex custom agent 기반의 읽기 전용 정적 분석 실습으로 변환함 |
| 원본 변경 여부 | 변경하지 않음 |

## 변환 내역

| 원본 구성 | Codex 실습 구성 | 처리 방식 |
|---|---|---|
| `CLAUDE.md` | `AGENTS.md` | 분석 규칙과 보고 기준으로 변환함 |
| `.claude/agents/security-analyst.md` | `.codex/agents/security-analyst.toml` | Codex custom agent 설정으로 변환함 |
| `sample-app/app.py`, `sample-app/db.py` | `sample-app/app.py`, `sample-app/db.py` | 사용자의 실습 요청에 따라 원본 학습 대상을 현재 실습 폴더에 재현함 |
| `result/security-report.md` | `result/` | 읽기 전용 agent는 자동 생성하지 않으며, 사용자가 채팅 결과를 직접 보관함 |

## 확인 필요 사항

- Codex 앱의 agent 선택 화면은 버전과 작업 폴더 상태에 따라 표시 위치가 달라질 수 있음.
- 외부 프로젝트에 적용할 때는 해당 프로젝트 루트의 `.codex/agents/`에 설정 파일을 배치한 뒤 새 task에서 확인 필요.
