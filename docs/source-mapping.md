# 원본 예제와 Codex 실습 구조 매핑

| 원본 구조 | Codex 실습 구조 | 변환 기준 |
|---|---|---|
| `CLAUDE.md` | `AGENTS.md` | 지속 적용할 프로젝트 규칙 정의 |
| `.claude/agents/` | 명시적 작업 절차 또는 Codex Skill | 반복 업무 역할과 절차 분리 |
| `.claude/skills/` | Codex Skill 또는 `AGENTS.md` 규칙 | 재사용 작업 흐름 정의 |
| `non-harness/` | `non-harness/` | 동일 요청의 기준 결과 보존 |
| `apply-harness/` | `apply-harness/` | 규칙 적용 결과 및 근거 보존 |
| 팁 인덱스 | `tips/README.md` | 결과 누적 및 재사용성 확인 |

## 확인 필요 사항

- 원본 예제에서 사용한 Claude Code 고유 기능은 Codex의 현재 설치 상태와 프로젝트 설정에 따라 대응 방식이 달라질 수 있음.
- 멀티에이전트, Hook, MCP는 단일 A/B 실험이 완료된 후 별도 예제로 분리하여 검증 필요.
