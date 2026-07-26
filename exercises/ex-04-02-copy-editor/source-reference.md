# 원본 예제 대응표

## 기준 자료

| 항목 | 내용 |
|---|---|
| 원본 경로 | `/Users/len/Downloads/harness-engineering-with-cc-main/ex-04-02-copy-editor/` |
| 변환 목적 | Claude Code 전용 agent 구성을 Codex custom agent 기반의 제한된 기계 교정 실습으로 변환함 |
| 원본 변경 여부 | 변경하지 않음 |

## 변환 내역

| 원본 구성 | Codex 실습 구성 | 처리 방식 |
|---|---|---|
| `CLAUDE.md` | `AGENTS.md` | 교정 범위, 예외, 보고 기준으로 변환함 |
| `.claude/agents/copy-editor.md` | `.codex/agents/copy-editor.toml` | Codex custom agent 설정으로 변환함 |
| `sample-doc/ch-07-draft.md` | `practice-target/ch-07-draft.md` | 원본을 복사하지 않고 독립적인 가상 원고로 작성함 |
| 자동 보고서 파일 | `result/` | 자동 생성하지 않음. 사용자가 채팅 결과를 직접 보관할 수 있음 |

## 확인 필요 사항

- Codex 앱의 agent 선택 화면은 버전과 작업 폴더 상태에 따라 표시 위치가 달라질 수 있음.
- 다른 원고에 적용할 때는 `.codex/agents/copy-editor.toml`을 해당 프로젝트 루트에 배치하고, 수정 허용 경로를 프로젝트 구조에 맞게 검토 필요.
