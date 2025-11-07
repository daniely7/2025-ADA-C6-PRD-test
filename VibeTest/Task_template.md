# Task_template.md — 앱별 개발 순서 템플릿

> **AI_PRD.md 문서를 기반으로 아래 작성 방법 및 문서 형식에 맞춰 Task.md 문서를 생성합니다.**
> **사용자가 지시하지 않은 추가 기능은 임의로 구현하지 않습니다.**
> **사용 시점**: 개발 시작부터 완료까지 지속적으로 참조  
> **용도**: 전체 개발 체크리스트 / **Task.json 작성에 필요** 


```
# 데일리 습관 트래커 개발 명세서

## 0단계: 프로젝트 세팅

### 작업 항목 (Task Breakdown)
- Xcode 프로젝트 생성
  - Xcode 프로젝트 생성여부 확인
      - 생성 완료: 다음 작업(아키텍처에 따른 폴더 구조 생성) 수행
      - 생성 미완료: Xcode 프로젝트 생성 가이드를 사용자에게 전달하고, 생성 완료를 기다린다

#### Xcode 프로젝트 생성 가이드

안녕하세요! 개발 시작 전에 먼저 아래와 같이 Xcode에서 프로젝트를 생성해 주시면 다음 단계를 진행할 수 있습니다.

1. **Xcode 실행** → "Create New Project" 선택
2. **iOS 탭** → "App" 선택 → Next
3. **프로젝트 설정:**
   - Product Name: `DailyHabitTracker`
   - Team: (본인 계정)
   - Organization Identifier: `com.yourname`
   - Interface: **SwiftUI** ✅
   - Language: **Swift** ✅
   - Storage: **Core Data** ✅
   - Include Tests: 선택 사항
4. **저장 위치:** 원하는 위치 선택 후 Create
5. **빌드 확인** (⌘ + B)

프로젝트 생성이 완료되면 **"생성 완료"**라고 답변해주세요.

### 완료 기준
- Xcode 프로젝트 생성 확인
- 모든 폴더 생성 완료
- 기존 Xcode 생성 파일들을 적절한 폴더로 이동
- 빌드 가능한 상태 확인

---

## 1단계: 데이터 모델링

### 작업 항목 (Task Breakdown)
- Habit Entity 정의 (Core Data)
  - id: UUID (Primary Key)
  - name: String (습관 이름)
  - emoji: String? (이모지, 선택 사항)
  - frequency: String (매일/주중/주말/주N회)
  - createdAt: Date (생성 날짜)
  - updatedAt: Date (수정 날짜)
  - habitRecords: Relationship (1:N, HabitRecord와 관계)
  - **수정 파일**:
    - `DailyHabitTracker.xcdatamodeld`
    - `Habit+CoreDataClass.swift`
    - `Habit+CoreDataProperties.swift`
  
- HabitRecord Entity 정의 (Core Data)
  - id: UUID (Primary Key)
  - date: Date (기록 날짜)
  - isCompleted: Bool (완료 여부)
  - habit: Relationship (N:1, Habit과 관계)
  - **수정 파일**:
    - `DailyHabitTracker.xcdatamodeld`
    - `HabitRecord+CoreDataClass.swift`
    - `HabitRecord+CoreDataProperties.swift`

- PersistenceController 구현
  - Core Data Stack 초기화
  - Preview용 In-Memory Store
  - 에러 핸들링
  - **수정 파일**:
    - `PersistenceController.swift`

- 데이터 검증 규칙 정의
  - 습관 이름 길이 제한 (1-30자)
  - 중복 이름 검증
  - 날짜 유효성 검증
  - **수정 파일**:
    - `ValidationRules.swift`

### 완료 기준 (Definition of Done)
- Habit, HabitRecord Entity 정의 완료
- Entity 속성 및 관계 설정 완료
- PersistenceController 구현 완료
- 데이터 무결성 규칙 정의 완료
- Core Data 모델 버전 관리 설정
- Preview용 샘플 데이터 생성 함수 구현

---

## 2단계: 기능 구현

### 작업 항목 (Task Breakdown)
- HabitService 로직 구현
  - 습관 추가 로직 (context의 insert 메서드)
  - 습관 수정 로직
  - 습관 삭제 로직 (context의 delete 메서드)
  - 습관 목록 조회 로직
  - 중복 검증 로직
  - **수정 파일**:
    - `HabitService.swift`

- HabitRecordService 로직 구현
  - 오늘 날짜 체크 생성 로직
  - 체크 상태 토글 로직
  - 특정 날짜 기록 조회 로직
  - 날짜별 완료율 계산 로직
  - **수정 파일**:
    - `HabitRecordService.swift`

- 통계 계산 로직
  - 연속 달성 일수 계산
  - 최고 연속 기록 계산
  - 전체/주간/월간 달성률 계산
  - TOP 3 습관 계산 (연속 일수, 달성률 기준)
  - 오늘 달성률 계산
  - **수정 파일**:
    - `StatisticsService.swift`

- 캘린더 데이터 로직
  - 월간 달성 현황 계산
  - 날짜별 완료 습관 수 계산
  - 완벽한 날 판단 로직
  - **수정 파일**:
    - `CalendarService.swift`

- 날짜 관리 로직
  - 오늘 날짜 판단 (Calendar의 isDateInToday 메서드)
  - 자정 기준 날짜 변경 감지
  - 날짜 비교 및 차이 계산
  - **수정 파일**:
    - `DateHelper.swift`

- 입력 검증 로직
  - 습관 이름 유효성 검증 (1-30자)
  - 중복 이름 검증
  - 빈 값 검증
  - **수정 파일**:
    - `ValidationHelper.swift`

- 데이터 저장 로직
  - Context 저장 (context의 save 메서드)
  - 에러 핸들링
  - 자동 저장 트리거
  - **수정 파일**:
    - `DataManager.swift`

### 완료 기준 (Definition of Done)
- 모든 CRUD 기능 구현 완료
- 통계 계산 로직 정확성 검증
- 연속 일수 계산 로직 검증
- 달성률 계산 로직 검증
- 입력 검증 로직 완성
- 데이터 저장/로딩 기능 완성
- 날짜 처리 로직 정확성 검증
- 단위 테스트 작성

---

## 3단계: UI 구현 및 기능 연결
> 3단계 Task 중 **아래 설정 규칙에 해당하는 Task는 작업 완료 후 다음 Task로 자동으로 넘어가지 않고, 사용자 테스트 확인을 받을 때까지 대기한다**

> **설정 규칙:**
- **UI 구현 단계 (milestone.type === "uiImplementation")** + **"View 개발" 또는 "화면 구현" 포함**
- 위 규칙에 포함되지 않는 Task는 사용자 테스트 확인 제외

> **테스트 체크리스트 작성 방법**
- 각 항목은 앱을 처음 실행하거나 최소한의 데이터만으로 즉시 확인 가능해야 함
- O/X로 답변이 가능한 질문이어야 함
- 전문 용어의 경우 일반 사용자가 이해할 수 있는 단어를 사용하여 표현할 것
  - 예시: 주간 습관 히트맵 -> 일주일 동안의 습관 달성 현황
- 구체적이고 명확한 표현을 사용할 것
- 첫번째 체크리스트는 "시뮬레이터 빌드가 잘 되나요?" 문장으로 고정

### 작업 항목 (Task Breakdown)
- MainTabView 개발
  - TabView 구조 구현
  - 습관 목록 탭
  - 통계 탭
  - 캘린더 탭
  - 탭 아이콘 및 레이블 설정
  - **수정 파일**:
    - `MainTabView.swift`
    - `DailyHabitTrackerApp.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 탭을 눌렀을 때 해당 화면으로 전환되나요?
    - 각 탭에 올바른 아이콘과 이름이 표시되나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- HabitListView 개발
  - FetchRequest로 습관 목록 자동 바인딩
  - Environment의 managedObjectContext로 컨텍스트 주입
  - 오늘 날짜 표시
  - 습관 리스트 표시 및 정렬
  - 각 습관 행 UI (체크박스, 이름, 연속일수, 달성률)
  - 체크박스 탭 기능 연동 및 HabitRecordService 호출
  - 스와이프 삭제 제스처 및 삭제 기능 연동
  - 플러스 버튼 및 추가 화면 네비게이션
  - 빈 상태 UI (습관이 없을 때)
  - **수정 파일**:
    - `HabitListView.swift`
    - `HabitRowView.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 오늘 날짜가 정확하게 표시되나요?
    - 습관 목록이 제대로 보이나요?
    - 체크박스를 누르면 체크 상태가 변경되나요?
    - 스와이프로 습관을 삭제할 수 있나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- HabitRowView 개발
  - 체크박스 UI 및 체크 상태 표시
  - 습관 이름 및 이모지 표시
  - 연속 달성 일수 표시 (🔥 아이콘 포함)
  - 달성률 표시 및 색상 구분
  - 탭 제스처로 상세/수정 화면 연동
  - 완료 상태에 따른 시각적 변화
  - **수정 파일**:
    - `HabitRowView.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 체크박스가 정상적으로 표시되나요?
    - 체크 여부에 따라 습관이 구분되어 보이나요?
    - 습관 이름과 이모지가 보이나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- AddHabitView 개발
  - 습관 이름 입력 필드 및 검증 로직 연동
  - 이모지 선택 UI
  - 목표 빈도 선택 Picker
  - 저장 버튼 및 HabitService의 create 메서드 호출
  - 취소 버튼
  - 입력 검증 에러 메시지 표시
  - **수정 파일**:
    - `AddHabitView.swift`
    - `HabitService.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 습관 이름을 입력할 수 있나요?
    - 이모지를 선택할 수 있나요?
    - 목표 빈도를 선택할 수 있나요?
    - 저장 버튼을 누르면 새 습관이 추가되나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- EditHabitView 개발
  - 기존 습관 데이터 로딩
  - 수정 가능 필드 표시
  - 저장 버튼 및 HabitService의 update 메서드 호출
  - 취소 버튼
  - 삭제 버튼 및 확인 알림
  - **수정 파일**:
    - `EditHabitView.swift`
    - `HabitService.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 기존 습관 정보가 제대로 표시되나요?
    - 정보를 수정할 수 있나요?
    - 저장 버튼을 누르면 변경사항이 저장되나요?
    - 삭제 버튼을 누르면 확인 알림이 나타나나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- StatisticsView 개발
  - 전체 통계 카드 표시
  - 오늘 달성률 표시 및 계산 로직 연동
  - 주간 달성률 표시 및 계산 로직 연동
  - 월간 달성률 표시 및 계산 로직 연동
  - TOP 3 연속 기록 표시 및 계산 로직 연동
  - TOP 3 달성률 표시 및 계산 로직 연동
  - 주간 히트맵 표시
  - **수정 파일**:
    - `StatisticsView.swift`
    - `StatisticsService.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 오늘/주간/월간 달성률 영역이 표시되나요?
    - 연속 기록 영역이 표시되나요?
    - 일주일 동안의 습관 달성 현황이 보이나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- CalendarView 개발
  - 월간 캘린더 그리드 구현
  - 각 날짜에 달성 현황 표시 및 계산 로직 연동
  - 날짜별 색상 구분 (완벽/부분/미달성)
  - 날짜 탭 기능 및 상세 기록 연동
  - 월 이동 버튼 (이전/다음)
  - **수정 파일**:
    - `CalendarView.swift`
    - `CalendarService.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 월간 캘린더가 제대로 표시되나요?
    - 날짜별로 달성 현황이 보이나요?
    - 날짜를 탭하면 상세 정보가 나타나나요?
    - 이전/다음 버튼으로 월을 이동할 수 있나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- DayDetailView 개발
  - 선택한 날짜 표시
  - 해당 날짜의 습관 체크 현황 표시
  - 완료/미완료 습관 리스트
  - **수정 파일**:
    - `DayDetailView.swift`
  - **테스트 체크리스트**:
    - 시뮬레이터 빌드가 잘 되나요?
    - 선택한 날짜가 정확하게 표시되나요?
    - 해당 날짜의 습관 현황이 보이나요?
    - 완료/미완료 습관이 구분되어 표시되나요?
    > 🛑 **중단점**: 작업 완료 후 사용자 테스트 확인 대기

- HabitViewModel 구현
  - 습관 관련 비즈니스 로직 캡슐화
  - 통계 계산 로직 호출
  - 사용자 액션 처리
  - 데이터 저장 트리거
  - **수정 파일**:
    - `HabitViewModel.swift`

- 애니메이션 적용
  - 체크박스 토글 애니메이션
  - 습관 추가/삭제 애니메이션
  - 달성률 변경 애니메이션
  - 연속 일수 증가 애니메이션
  - **수정 파일**:
    - `AnimationHelper.swift`

### 완료 기준 (Definition of Done)
- 사후 검증 규칙(아래 명시) 통과
  - 2단계(기능 구현)에서 개발된 모든 로직이 3단계(UI 구현 및 기능 연결)에서 실제로 연동되는지 확인
  - 누락된 UI 연동이 있다면 3단계에 추가
  - 검증 방법: 2단계의 각 로직마다 "이 기능을 호출하는 UI 컴포넌트가 3단계에 명시되어 있는가?" 확인
- 모든 사용자 플로우 구현 완료
- 습관 CRUD 기능 정상 작동
- 체크 기능 정상 작동 및 실시간 반영
- 통계 계산 정확도 검증
- 캘린더 표시 정확도 검증
- 입력 검증 정상 작동
- 데이터 바인딩 정상 작동
- 애니메이션 자연스럽게 작동
- 색상 코드 정확하게 표시
- 사용성 테스트 통과

```
