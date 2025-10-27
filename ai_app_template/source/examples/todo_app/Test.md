# Test.md — 테스트 전략 / 케이스 / DoD

## 1) 전략
- 계층별: Unit(UseCases/Repository) → UI 테스트(SwiftUI + A11y ID) → 수동 시나리오
- 핵심 플로우 우선: 추가 → 토글 → 삭제 → 편집 → 통계 → 영속성 재검증
- 회귀 방지: 각 기능 병합 시 전체 스위트 재실행

## 2) 자동화 테스트 케이스 (발췌)
### Unit — TodoService
- [ ] `add(title)` 빈 문자열 거부
- [ ] `add` 후 최신순 정렬 상단 삽입
- [ ] `toggle(id)` 완료/미완료 토글
- [ ] `edit(id,newTitle)` 트림 후 빈 값 거부
- [ ] `delete(id)` 정상 삭제
- [ ] `stats()` total/completed/active/completeRate 정확

### Unit — CoreDataTodoRepository
- [ ] CRUD 일관성 (저장→조회→수정→삭제)
- [ ] 앱 재실행(컨텍스트 재생성) 후 데이터 보존

### UI (A11y ID 이용)
- [ ] `field.input` 포커스 상태에서 입력 후 `btn.add` 탭 → `list.todo` 상단에 새 항목
- [ ] `row.checkbox` 탭 → 취소선/상태 반영
- [ ] 스와이프 삭제(`btn.delete`) → 리스트에서 제거
- [ ] 행 탭 후 인라인 편집(`btn.edit`) → 저장/ESC 취소
- [ ] 통계 섹션의 카운트/퍼센트 자동 갱신

## 3) 수동 시나리오
- [ ] 첫 실행 시 입력창이 **최상단**에 보임 (하단 배치 금지)
- [ ] 10개 연속 입력 시 타이핑 흐름 끊김 없음(자동 포커스 유지)
- [ ] 다중 토글/삭제 뒤에도 최신순 유지
- [ ] 다크모드/라이트모드 UI 파손 없음
- [ ] iPhone SE/Pro Max 레이아웃 문제 없음

## 4) 실패 기준/리포트
- **빌드 에러**: 0 (하나라도 있으면 FAIL)
- **크래시**: 0
- 실패 로그는 `{"phase":"test","taskId":"TX","status":"error","details":"..."}` 로 남김

## 5) DoD(Definition of Done)
- [ ] 자동/수동 테스트 모두 통과
- [ ] 데이터 영속(재실행 후 항목 유지) 검증
- [ ] A11y ID 전 항목 부여 및 UI 테스트 통과
- [ ] 레이어 의존 규칙 준수 확인
- [ ] `stdout` 최종 요약 JSON 출력

---

## 6) 운영 메모(곰민 인터뷰 반영)
- **가시성**: 각 Task 시작/종료/ETA 로그 필수(JSONL)
- **신뢰맞춤**: AI “완료”라도 테스트 미통과 시 DONE 금지
- **질문-재확인**: 위험추정 시 BLOCKER 보고 + 제안(최소 1안)