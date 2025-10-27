# F01_AddTodo — 상단 입력으로 즉시 추가

## 목적
사용자가 **상단 입력창**에서 빠르게 항목을 추가한다.

## 유저 스토리
- 나는 해야 할 일을 떠올리는 즉시 맨 위 입력창에서 바로 추가하고 싶다.

## 유저 플로우
1) 앱 실행 → 상단 입력창(`field.input`) 자동 포커스  
2) 텍스트 입력 → `btn.add` 또는 키보드 Return  
3) 리스트(`list.todo`) **상단**에 새 항목 표시 → 입력창 비우고 포커스 유지

## UI-CONTRACT
- 상단 고정: `inputSection` → `statisticsSection` → `todoListSection`
- A11y: `field.input`, `btn.add`, `list.todo`
- 비어있으면 `btn.add` 비활성

## 데이터/규칙
- title 트림 후 공백이면 추가 금지
- 생성 시점: `createdAt = now`, `updatedAt = createdAt`, `isCompleted = false`
- 정렬: `createdAt` 내림차순

## 오류/엣지
- 매우 긴 텍스트: 200자 제한, 초과 시 사용자 경고(토스트 또는 Alert)

## 완수 기준(DoD)
- [ ] 상단 입력창에서 3건 연속 추가 시 포커스 유지
- [ ] 빈 입력 추가 시도 → 방지 동작 확인
- [ ] 새 항목이 항상 리스트 **최상단**에 위치
- [ ] Unit+UI 테스트 통과