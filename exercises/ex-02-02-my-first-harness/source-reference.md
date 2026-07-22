# 원본 자료 변환 기준

## 기준 자료

| 항목 | 내용 |
|---|---|
| 원본 경로 | `/Users/len/Downloads/harness-engineering-with-cc-main/ex-02-02-my-first-harness/` |
| 변환 목적 | Claude Code 전용 최소 하네스를 Codex 프로젝트 로컬 설정으로 변환함 |
| 원본 처리 | 원본 자료와 다운로드 디렉터리는 수정하지 않음 |

## 변환 매핑

| 원본 | Codex 변환 |
|---|---|
| `CLAUDE.md` | `AGENTS.md` |
| `.claude/agents/*.md` | `.codex/agents/*.toml` |
| `.claude/skills/commit-message/SKILL.md` | `.agents/skills/commit-message/SKILL.md` |

## 범위 제외

- Claude Code 전용 `.claude/` 설정 파일과 `CLAUDE.md`를 복사하지 않음.
- 실제 커밋, 원격 푸시, PR 생성을 포함하지 않음.
