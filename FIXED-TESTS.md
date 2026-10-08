# T05 고정 검사 10개 — 작업 시작 전 등록

등록 시각: 2026-10-08 KST (작업 시작 전)
적용 대상 저장소: `sterran123/daily-signal-board`
검사 파일: `t05-fixed.test.cjs` (저장소 루트)
실행 명령: `node --test t05-fixed.test.cjs`

개선 기능(선언한 기능 한 개): **공유 기록 표에 지역별 최저·최고·전일 대비 추세 요약을 추가한다.**
구현 대상 함수는 `core.js`의 `sharedStatsFor(readings, signalId)`와 `sharedStatsCells(stats)`이고,
`app.js`가 공유 기록 표에 결과 컬럼을 렌더링한다.

| ID | 입력 | 관찰 가능한 기대값 |
| --- | --- | --- |
| F01 | readings=[{signal_id:'s',record_date:'2026-10-07',normalized_value:18.3,unit:'°C'},{signal_id:'s',record_date:'2026-10-08',normalized_value:20.6,unit:'°C'}], signalId='s' | `sharedStatsFor(...).count` = `2` |
| F02 | F01과 동일 | `min` = `18.3` |
| F03 | F01과 동일 | `max` = `20.6` |
| F04 | F01과 동일 | `latest` = `20.6` |
| F05 | F01과 동일 | `delta` = `{direction:'increase', magnitude:2.3, unit:'°C'}` (magnitude ±0.001) |
| F06 | readings=[10.0(10-07), 8.0(10-08)] 같은 단위 '°C' | `delta.direction` = `'decrease'`, `magnitude` = `2` |
| F07 | readings=[7.5(10-07), 7.5(10-08)] | `delta.direction` = `'unchanged'`, `magnitude` = `0` |
| F08 | readings=[1건만(2026-10-08, 20.6, '°C')] | `delta.direction` = `'insufficient'` |
| F09 | F01 결과 stats → `sharedStatsCells(stats)` | `{min:'18.3°C', max:'20.6°C', trend:'▲ +2.3°C'}` |
| F10 | readings=[] → `sharedStatsFor` → `sharedStatsCells` | `{min:'—', max:'—', trend:'—'}` |

## 공통 사용 상한 (AI A·B 동일 적용, 작업 전 기록)

- 시간 상한: **AI별 실제 작업시간 ≤ 60분**
- 요청·호출 수 상한: **AI별 도구 호출(파일 읽기·편집·명령 실행 등 도구 호출 합계) ≤ 120회**

## 공통 최초 요청 (두 AI가 받는 동일 문장)

> daily-signal-board 저장소의 공유 기록 표에 지역별 최저·최고·전일 대비 추세 요약을 추가하라.
> 저장소에 고정된 `t05-fixed.test.cjs`의 검사 10개를 모두 통과시켜라.

AI B에게는 저장소와 인수인계 문서만 제공되며, 위 문장은 인수인계 문서의 "목표" 항목에 동일하게 포함된다.

## 시작 소스

- `daily-signal-board @ 9dc4e9b5985017eac43d94468c86f9a0f519d03f` (main, 작업 시작 시점 HEAD)
