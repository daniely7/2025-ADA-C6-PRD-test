# F02_ToggleComplete — 완료/미완료 토글

## 목적
목록에서 바로 완료 상태를 전환한다.

## 유저 플로우
1) 항목 행의 체크박스(`row.checkbox`) 탭  
2) 완료 시 취소선/감쇄 애니메이션  
3) 통계 자동 갱신

## UI-CONTRACT
- A11y: `row.checkbox`, `row.todo`
- 완료 상태 시 라벨 `strikethrough(true)`

## 데이터/규칙
- `isCompleted` 토글, `updatedAt = now`
- 정렬 정책은 **최신 생성순** 유지(완료 여부와 무관)

## 오류/엣지
- 빠른 연타에도 상태 일관성 유지(디바운스 또는 상태락)

## 완수 기준(DoD)
- [ ] 연속 토글 10회 이상 무결성
- [ ] 통계의 완료/미완료 수 즉시 반영
- [ ] Unit+UI 테스트 통과