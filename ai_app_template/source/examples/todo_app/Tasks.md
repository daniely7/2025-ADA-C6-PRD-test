---

**Tasks.md**

```markdown
# Tasks.md — 구현 순서(크리티컬 패스)

> 모든 단계는 콘솔에 PROGRESS 로그(JSONL) 출력. 실패 시 BLOCKER 리포트.

## 0. 프로젝트 설정 (단순화)
- [ ] **1단계 - 프로젝트 감지**: 기존 Xcode 프로젝트 존재 여부 확인
  - [ ] .xcodeproj 파일 검색
  - [ ] 있으면 실제 프로젝트명 사용, 없으면 AI_OnePageTodo 사용
- [ ] **2단계 - 설정 확인** (기존 프로젝트가 있는 경우만): SwiftUI, iOS 16.0+, Core Data 포함 여부 검증
- [ ] **3단계 - 구조 설정**: 폴더 생성 ($ACTUAL_PROJECT_NAME 사용)
  - [ ] Presentation/Views, Presentation/ViewModels
  - [ ] Domain/{Entities,UseCases,RepositoryInterfaces}
  - [ ] Data/{Repositories,CoreData}
- [ ] **4단계 - 빌드 확인** (기존 프로젝트가 있는 경우만): 기본 스킴으로 빌드 테스트

## 1. 데이터 모델링 (Domain + CoreData)
- [ ] Domain/Entities/TodoItem.swift: `id(UUID)`, `title(String)`, `isCompleted(Bool)`, `createdAt(Date)`, `updatedAt(Date)`
- [ ] Domain/RepositoryInterfaces/TodoRepository.swift (protocol): CRUD, fetch(sort: 최신순)
- [ ] Data/CoreData: `TodoModel.xcdatamodeld` 에 동일 스키마
- [ ] Data/PersistenceController.swift: 컨테이너/컨텍스트 구성

## 2. 저장소 + 유스케이스
- [ ] Data/Repositories/CoreDataTodoRepository.swift: Repository 구현
- [ ] Domain/UseCases/TodoService.swift:
  - add(title)
  - toggle(id)
  - delete(id)
  - edit(id, newTitle)
  - stats() — total/completed/active, rate

## 3. ViewModel
- [ ] Presentation/ViewModels/TodoViewModel.swift:
  - state: `input`, `todos: [TodoItem]`, `stats`
  - intents: `onAdd`, `onToggle`, `onDelete`, `onEdit`, `onChangeInput`
  - lifecycle: 초기 fetch, 변경 시 퍼블리시

## 4. UI (SwiftUI)
- [ ] Presentation/Views/ContentView.swift:
  - 상단 `TextField(field.input)` + `Button(btn.add)`
  - 중간 통계 섹션
  - 하단 리스트(`list.todo`) — `TodoRowView`
- [ ] Presentation/Views/TodoRowView.swift:
  - 체크박스(`row.checkbox`) 토글
  - 스와이프 삭제(`btn.delete`)
  - 탭 인라인 편집(`btn.edit`)

## 5. 접합/정리
- [ ] 의존성 방향 점검 (Presentation → Domain → Data)
- [ ] 파일 경로/그룹 구조 재검증
- [ ] A11y ID 최종 확인

## 6. 테스트/품질
- [ ] Unit: Repository, Service
- [ ] UI: 핵심 플로우(추가/토글/삭제/편집), A11y ID 기반
- [ ] 수동 체크리스트: Test.md 기준 전부 체크

## 7. 산출/리포트
- [ ] `stdout`에 최종 요약(JSON): 통과/실패, 테스트 개수, 경과시간