# sample-app — 의도적 취약 Flask 앱

> **경고: 절대 배포하지 말 것.** `security-analyst` agent의 분석 대상으로만 사용하는 의도적 취약 코드임.

## 점검 대상

| No | 취약점 유형 | 관련 파일 | OWASP 예시 |
|---:|---|---|---|
| 1 | SQL Injection | `app.py` | A03:2021 |
| 2 | 하드코딩 자격증명 | `app.py` | A05/A07:2021 |
| 3 | 약한 해시(MD5) | `app.py` | A02:2021 |
| 4 | XSS | `app.py` | A03:2021 |
| 5 | IDOR·인가 누락 | `app.py` | A01:2021 |
| 6 | 디버그 설정 | `app.py` | A05:2021 |

`db.py`는 `app.py`가 호출하는 DB 헬퍼이므로 함께 읽기 전용으로 검토 필요.
