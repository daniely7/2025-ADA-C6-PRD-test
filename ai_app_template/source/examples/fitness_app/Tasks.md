# Tasks.md — 구현 순서(크리티컬 패스)

> 모든 단계는 콘솔에 PROGRESS 로그(JSONL) 출력. 실패 시 BLOCKER 리포트.

## 0. 프로젝트 설정 (단순화)
- [ ] **1단계 - 프로젝트 감지**: 기존 Xcode 프로젝트 존재 여부 확인
  - [ ] .xcodeproj 파일 검색
  - [ ] 있으면 실제 프로젝트명 사용, 없으면 AI_FitnessTracker 사용
- [ ] **2단계 - 설정 확인** (기존 프로젝트가 있는 경우만): SwiftUI, iOS 16.0+, Core Data 포함 여부 검증
- [ ] **3단계 - 구조 설정**: 폴더 생성 ($ACTUAL_PROJECT_NAME 사용)
  - [ ] Presentation/Views, Presentation/ViewModels
  - [ ] Domain/{Entities,UseCases,RepositoryInterfaces}
  - [ ] Data/{Repositories,CoreData}
- [ ] **4단계 - 빌드 확인** (기존 프로젝트가 있는 경우만): 기본 스킴으로 빌드 테스트

## 1. 데이터 모델링 (Domain + CoreData)
- [ ] Domain/Entities/WorkoutRecord.swift: `id(UUID)`, `exercise(String)`, `sets(Int32)`, `reps(Int32)`, `weight(Double)`, `createdAt(Date)`
- [ ] Domain/RepositoryInterfaces/WorkoutRepository.swift (protocol): CRUD, fetch(sort: 최신순)
- [ ] Data/CoreData: `FitnessModel.xcdatamodeld` 에 동일 스키마
- [ ] Data/PersistenceController.swift: 컨테이너/컨텍스트 구성

## 2. 저장소 + 유스케이스
- [ ] Data/Repositories/CoreDataWorkoutRepository.swift: Repository 구현
- [ ] Domain/UseCases/WorkoutService.swift:
  - addWorkout(exercise, sets, reps, weight)
  - getWorkouts()
  - deleteWorkout(id)
  - updateWorkout(id, newData)
  - getWorkoutStats()

## 3. ViewModel
- [ ] Presentation/ViewModels/WorkoutViewModel.swift:
  - state: workouts, isLoading, selectedWorkout
  - intents: add, delete, update, refresh
  - lifecycle: 초기 fetch, 변경 시 퍼블리시

## 4. UI (SwiftUI)
- [ ] Presentation/Views/ContentView.swift:
  - 운동 기록 입력 섹션
  - 최근 운동 목록
  - 통계 요약 섹션
- [ ] Presentation/Views/WorkoutRecordRowView.swift:
  - 운동 정보 표시
  - 편집/삭제 기능
  - 세트/무게/횟수 표시

## 5. 접합/정리
- [ ] 의존성 방향 점검 (Presentation → Domain → Data)
- [ ] 파일 경로/그룹 구조 재검증
- [ ] A11y ID 최종 확인

## 6. 테스트/품질
- [ ] Unit: Repository, Service
- [ ] UI: 핵심 플로우(운동 추가/조회/수정/삭제), A11y ID 기반
- [ ] 수동 체크리스트: Test.md 기준 전부 체크

## 7. 산출/리포트
- [ ] `stdout`에 최종 요약(JSON): 통과/실패, 테스트 개수, 경과시간