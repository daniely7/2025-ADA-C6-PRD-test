# Tasks.md — 구현 순서(크리티컬 패스)

> 모든 단계는 콘솔에 PROGRESS 로그(JSONL) 출력. 실패 시 BLOCKER 리포트.

## 0. 프로젝트 설정 (단순화)
- [ ] **1단계 - 프로젝트 감지**: 기존 Xcode 프로젝트 존재 여부 확인
  - [ ] .xcodeproj 파일 검색
  - [ ] 있으면 실제 프로젝트명 사용, 없으면 AI_{{APP_NAME}} 사용
- [ ] **2단계 - 설정 확인** (기존 프로젝트가 있는 경우만): SwiftUI, iOS 16.0+, Core Data 포함 여부 검증
- [ ] **3단계 - 구조 설정**: 폴더 생성 ($ACTUAL_PROJECT_NAME 사용)
  - [ ] Presentation/Views, Presentation/ViewModels
  - [ ] Domain/{Entities,UseCases,RepositoryInterfaces}
  - [ ] Data/{Repositories,CoreData}
- [ ] **4단계 - 빌드 확인** (기존 프로젝트가 있는 경우만): 기본 스킴으로 빌드 테스트

## 1. 데이터 모델링 (Domain + CoreData)
- [ ] Domain/Entities/{{MAIN_ENTITY}}.swift: {{ENTITY_PROPERTIES}}
- [ ] Domain/RepositoryInterfaces/{{MAIN_REPOSITORY}}.swift (protocol): {{REPOSITORY_METHODS}}
- [ ] Data/CoreData: `{{DATA_MODEL}}.xcdatamodeld` 에 동일 스키마
- [ ] Data/PersistenceController.swift: 컨테이너/컨텍스트 구성

## 2. 저장소 + 유스케이스
- [ ] Data/Repositories/CoreData{{MAIN_REPOSITORY}}.swift: Repository 구현
- [ ] Domain/UseCases/{{MAIN_SERVICE}}.swift:
  - {{SERVICE_METHOD_1}}
  - {{SERVICE_METHOD_2}}
  - {{SERVICE_METHOD_3}}
  - {{SERVICE_METHOD_4}}
  - {{SERVICE_METHOD_5}}

## 3. ViewModel
- [ ] Presentation/ViewModels/{{MAIN_VIEW_MODEL}}.swift:
  - state: {{STATE_PROPERTIES}}
  - intents: {{INTENT_METHODS}}
  - lifecycle: 초기 fetch, 변경 시 퍼블리시

## 4. UI (SwiftUI)
- [ ] Presentation/Views/ContentView.swift:
  - {{UI_SECTION_1}}
  - {{UI_SECTION_2}}
  - {{UI_SECTION_3}}
- [ ] Presentation/Views/{{MAIN_ROW_VIEW}}.swift:
  - {{ROW_FEATURE_1}}
  - {{ROW_FEATURE_2}}
  - {{ROW_FEATURE_3}}

## 5. 접합/정리
- [ ] 의존성 방향 점검 (Presentation → Domain → Data)
- [ ] 파일 경로/그룹 구조 재검증
- [ ] A11y ID 최종 확인

## 6. 테스트/품질
- [ ] Unit: Repository, Service
- [ ] UI: 핵심 플로우({{CORE_UI_FLOWS}}), A11y ID 기반
- [ ] 수동 체크리스트: Test.md 기준 전부 체크

## 7. 산출/리포트
- [ ] `stdout`에 최종 요약(JSON): 통과/실패, 테스트 개수, 경과시간