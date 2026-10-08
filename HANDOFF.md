# 인수인계 문서 — daily-signal-board 공유 기록 요약 (AI A → AI B)

## 1. 목표

> daily-signal-board 저장소의 공유 기록 표에 지역별 최저·최고·전일 대비 추세 요약을 추가하라.
> 저장소에 고정된 `t05-fixed.test.cjs`의 검사 10개를 모두 통과시켜라.

## 2. 현재 상태

- 작업 저장소: `https://github.com/sterran123/daily-signal-board`
- 인수인계 기준 버전 ID: `a2880a598111a5fbf2731b8433aea01a22804022` (main HEAD)
- 구현됨: `core.js`에 `sharedStatsFor(readings, signalId)` — 지역별 count·min·max·latest·unit·delta(direction/magnitude/unit)를 계산한다. 반환 delta.direction 값은 `increase | decrease | unchanged | insufficient | unit_mismatch`.
- 미구현: `core.js`의 `sharedStatsCells(stats)`는 스텁이며 현재 TypeError를 던진다. `app.js`는 이 기능과 아직 연결되지 않았다.
- 저장소는 순수 HTML/CSS/JavaScript 정적 앱이다. 빌드·패키지 설치 없음.

## 3. 실행 명령

```powershell
git clone https://github.com/sterran123/daily-signal-board.git
cd daily-signal-board
git checkout a2880a598111a5fbf2731b8433aea01a22804022   # 인수인계 기준 버전 확인
node --test t05-fixed.test.cjs    # 고정 검사 10개 실행
node --test core.test.cjs         # 기존 회귀 검사 12개 실행
node serve.mjs                    # 브라우저 수동 확인용 로컬 서버 (선택)
```

필요 환경: Node.js 18 이상 (node:test 내장 러너 사용). npm install 불필요.

## 4. 통과 검사

- 고정 검사 10개 중 **F01~F08 통과, F09·F10 실패** (AI A 종료 시점)
- 기존 `core.test.cjs` 12개 전부 통과 — 이 상태를 유지해야 함

## 5. 남은 문제

- F09/F10 실패 원인: `sharedStatsCells` 미구현
- 공유 기록 표(`app.js`의 `renderSharedBoard`)에 최저·최고·추세 컬럼이 아직 없음
- 셀 문자열 규격(고정 검사 기대값): `{min:'18.3°C', max:'20.6°C', trend:'▲ +2.3°C'}` 형태. 추세는 increase `▲ +값`, decrease `▼ -값`, unchanged `– 0`, 그 외(insufficient·unit_mismatch·기록 없음) `—`. 단위는 값 뒤에 붙임(공백 없음). stats가 null이거나 count=0이면 세 셀 모두 `—`.

## 6. 다음 행동

1. `core.js`의 `sharedStatsCells(stats)`를 위 규격대로 구현한다 (순수 함수, DOM 사용 금지).
2. `app.js` 공유 기록 표에 지역별 `최저`·`최고`·`전일 대비` 컬럼을 렌더링해 기능을 완성한다.
3. `node --test t05-fixed.test.cjs`로 10/10 통과를 확인하고 `core.test.cjs`도 12/12 유지를 확인한다.
4. 커밋 후 `main`에 푸시하고, 종료 소스 버전 ID·소요 시간·도구 호출 수·검사 실행 결과를 보고 저장소에 기록한다.

## 7. 건드리지 말 것

- `t05-fixed.test.cjs` — 고정 검사다. 테스트 삭제·완화·기대값 변경 금지.
- `core.test.cjs` 기존 12개 검사 — 수정·삭제 금지.
- `data/daily.json`, `scripts/fetch-shared.mjs`, `.github/workflows/` — 수집 워크플로가 관리하는 영역.
- `assets/studio-task-assets/`, `고정 자산` 계열 파일 — 과제 고정 자산.
- `localStorage` 키 이름과 정규화 스키마(`NORMALIZED_KEYS`) — 제출 계약과 연결됨.
