# 목표와 완료 기준을 함께 요청하기

## 상황

수정 범위와 완료 상태가 불명확한 작업에 적용함.

## 방법

요청문에 수행할 변경 목표와 완료로 판단할 조건을 함께 작성함. 필요 시 관련 검증 실행도 완료 조건에 포함함.

## 예시

```text
로그인 오류를 수정하고 관련 테스트를 실행해 주세요.
오류가 재현되지 않으면 완료로 알려주세요.
```

## 주의점

완료 조건은 실제로 확인 가능한 상태로 작성 필요함. 확인할 수 없는 조건은 별도 확인 필요 사항으로 남기도록 요청 필요함.

## 출처

- OpenAI, [Codex Best practices](https://learn.chatgpt.com/guides/best-practices.md) — Goal, Context, Constraints, Done when 항목 안내
- OpenAI, [Prompting](https://learn.chatgpt.com/docs/prompting.md) — 목표·출력·경계 조건 및 최종 점검 안내
