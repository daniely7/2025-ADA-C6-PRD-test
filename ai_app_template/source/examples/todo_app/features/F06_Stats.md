# F06_Stats — 통계 섹션

## 목적
진행 현황을 한눈에 확인한다(전체/완료/미완료/완료율).

## 표시 규칙
- 상단 입력 아래, 리스트 위
- 형식 예: `전체 12 • 완료 7 • 미완료 5 • 58%`

## 데이터
- `total = n`, `completed`, `active = total-completed`
- `rate = floor(completed/total*100)` (total=0이면 0%)

## 업데이트
- add/toggle/delete/edit 후 즉시 재계산

## 완수 기준(DoD)
- [ ] 경계값(total=0, all completed) 정확
- [ ] 동시 변경 다발에도 값 오염 없음
- [ ] Unit+UI 테스트 통과