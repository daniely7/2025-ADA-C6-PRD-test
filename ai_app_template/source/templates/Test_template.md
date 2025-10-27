# Test.md — 테스트 전략 / 케이스 / DoD

## 1) 전략
- 계층별: Unit(UseCases/Repository) → UI 테스트(SwiftUI + A11y ID) → 수동 시나리오
- 핵심 플로우 우선: {{CORE_TEST_FLOWS}}
- 회귀 방지: 각 기능 병합 시 전체 스위트 재실행

## 2) 자동화 테스트 케이스 (발췌)
### Unit — {{MAIN_SERVICE}}
- [ ] {{UNIT_TEST_1}}
- [ ] {{UNIT_TEST_2}}
- [ ] {{UNIT_TEST_3}}
- [ ] {{UNIT_TEST_4}}
- [ ] {{UNIT_TEST_5}}

### Unit — CoreData{{MAIN_REPOSITORY}}
- [ ] CRUD 일관성 (저장→조회→수정→삭제)
- [ ] 앱 재실행(컨텍스트 재생성) 후 데이터 보존

### UI (A11y ID 이용)
- [ ] {{UI_TEST_1}}
- [ ] {{UI_TEST_2}}
- [ ] {{UI_TEST_3}}
- [ ] {{UI_TEST_4}}
- [ ] {{UI_TEST_5}}

## 3) 수동 시나리오
- [ ] {{MANUAL_TEST_1}}
- [ ] {{MANUAL_TEST_2}}
- [ ] {{MANUAL_TEST_3}}
- [ ] {{MANUAL_TEST_4}}

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

## 6) 운영 메모
- **가시성**: 각 Task 시작/종료/ETA 로그 필수(JSONL)
- **신뢰맞춤**: AI "완료"라도 테스트 미통과 시 DONE 금지
- **질문-재확인**: 위험추정 시 BLOCKER 보고 + 제안(최소 1안)