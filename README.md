# ai-handoff-report — 대화가 끊겨도 이어지는 프로젝트 (T05)

과제 4의 `daily-signal-board`에 작은 개선 하나를 두 AI 세션이 차례로 이어 완성한 과정을 기록한
무로그인 공개 비교 보고서입니다.

- 작업 저장소: https://github.com/sterran123/daily-signal-board
- 공개 보고서: https://sterran123.github.io/ai-handoff-report/

## 구조

- `index.html` — 공개 비교 보고서 (GitHub Pages로 배포)
- `FIXED-TESTS.md` — 작업 전에 고정한 검사 10개(ID·입력·기대값)와 공통 사용 상한
- `HANDOFF.md` — AI A가 남긴 7항목 인수인계 문서 (AI B에게 그대로 전달)
- `WORKLOG.md` — A 시작 → A 인계 → B 시작 → B 완료의 작업 기록과 사용량

## 고정 규칙 요약

- 검사 10개·시간 상한 60분·호출 상한 120회는 작업 시작 전에 고정했다.
- AI B에는 작업 저장소와 인수인계 문서만 제공하고 대화 전문은 제공하지 않는다.
- 비교표의 모델·서비스 이름은 가리고 시간·호출 수·오류 회차·통과 수만 비교한다.
