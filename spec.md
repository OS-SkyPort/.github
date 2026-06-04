# SPEC — SkyPort

## 개요

공항 카운터 5대(C1 FIRST / C2 BUSINESS / C3 ECONOMY / C4·C5 FLEX)의 비선점형 다중 프로세서 스케줄링 시뮬레이터. 유일한 성능 지표는 평균 반환 시간(ATT).

## 범위

- FCFS·Fixed-Priority·SJF·HybridMLQ 비교, CLI·정적 웹 출력.
- 제외: 최적해·하한 계산, 선점, 카운터 속도 차등화, 데스크톱 GUI.

## 요구사항

- Python 3.10+ 표준 라이브러리만 사용. 예외: 테스트 `pytest`, 논문 그림 `matplotlib`.
- 입력은 `id, arrival_time, class, service_time` 4열. 공백/CSV, 헤더·빈 줄·`#` 주석 허용. 등급은 1/2/3 또는 이름, 숫자 ID는 `P01` 형식으로 정규화.
- 1 tick = 1 시간 단위. **도착 → 완료 → 유휴 카운터 배정 → 남은 시간 감소** 순서이며, 같은 t에 빈 카운터는 즉시 배정한다.
- 시작한 서비스는 완료까지 유지한다. 도착 전 승객은 선택하지 않는다. 동률의 최종 키는 `passenger_id`이며 같은 입력은 같은 결과를 낸다.
- `SAFETY_LIMIT = 1000` tick 초과 시 예외. `counter.kind`는 선호도이며 HybridMLQ만 참조한다.
- GUI는 frozen `SimSnapshot` 복사본만 읽고 엔진 내부 상태를 수정하지 않는다.

## 완료 기준

- 테스트로 입력 처리·비선점·도착 시각·결정성을 확인한다.
- 동일 입력의 모든 스케줄러 결과와 ATT를 비교·출력한다.
