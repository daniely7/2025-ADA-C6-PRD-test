# F01: 운동 기록 관리 (WorkoutRecord)

## 📋 기능 개요
헬스장에서 실시간으로 운동 세트를 빠르게 기록하고 관리하는 핵심 기능입니다. 사용자가 운동 중에 중단 없이 세트별 무게와 횟수를 기록할 수 있도록 최적화된 UI를 제공합니다.

## 🎯 사용자 스토리
```
As a 헬스장 이용자
I want to 운동 중에 빠르게 세트를 기록하고 싶어
So that 운동에 집중하면서도 정확한 기록을 남길 수 있어
```

## ⚡ 핵심 요구사항

### 🏋️‍♂️ 운동 추가 및 선택
- **빠른 운동 추가**: 상단 "+" 버튼으로 새 운동 즉시 추가
- **자동완성 지원**: 기존 운동명 타이핑 시 자동완성 제안
- **카테고리 자동 분류**: 운동명 기반으로 상체/하체/전신 자동 분류
- **최근 운동 우선**: 최근 7일 내 기록한 운동을 상단에 우선 표시

### 💪 세트 기록 입력
- **빠른 세트 추가**: 운동 선택 후 "세트 추가" 버튼으로 즉시 입력 모드
- **스마트 기본값**: 이전 세트의 무게/횟수를 기본값으로 자동 설정
- **수치 입력 최적화**: 무게는 2.5kg 단위, 횟수는 1 단위로 +/- 버튼 제공
- **한 번에 완료**: 무게→횟수→완료를 3탭 이내로 처리

### 📊 실시간 피드백
- **즉시 볼륨 계산**: 세트 완료 시 볼륨(무게×횟수) 즉시 표시
- **PR 감지**: 개인 기록 갱신 시 금색 배지와 축하 애니메이션
- **진행 상태 표시**: 총 세트 중 현재 몇 번째 세트인지 명확 표시
- **시각적 완료 표시**: 완료된 세트는 체크마크와 색상 변경

## 🎨 UI/UX 설계

### 📱 메인 화면 레이아웃
```
┌─────────────────────────────────┐
│  🏋️ FitnessTracker        [+]  │  ← 상단 고정 헤더
├─────────────────────────────────┤
│                                 │
│  📋 오늘의 운동                 │  ← 운동 목록 영역
│  ┌─ 벤치프레스 ──────────────┐  │
│  │ 3세트 완료 • 총 1,920kg  │  │
│  │ 80kg×8  80kg×7  75kg×8   │  │
│  └─────────────────────────┘  │
│                                 │
│  ┌─ 스쿼트 ────────── [진행중] ┐ │
│  │ 2/4세트 • 1,200kg        │  │
│  │ 100kg×10  95kg×8  [입력중] │ │
│  └─────────────────────────┘  │
│                                 │
├─────────────────────────────────┤
│ [운동 추가] [통계 보기] [설정]  │  ← 하단 고정 액션바
└─────────────────────────────────┘
```

### ⌨️ 세트 입력 화면
```
┌─────────────────────────────────┐
│  ← 스쿼트 • 3번째 세트           │
├─────────────────────────────────┤
│                                 │
│           무게 (kg)             │
│     ┌─┐  ┌─────────┐  ┌─┐      │
│     │-│  │  100.0  │  │+│      │  ← 2.5kg 단위 조정
│     └─┘  └─────────┘  └─┘      │
│                                 │
│            횟수                 │
│     ┌─┐  ┌─────────┐  ┌─┐      │
│     │-│  │   10    │  │+│      │  ← 1회 단위 조정
│     └─┘  └─────────┘  └─┘      │
│                                 │
│     이전: 100kg × 10회           │  ← 참고 정보
│     볼륨: 1,000kg               │
│                                 │
│  ┌─────────────────────────────┐ │
│  │        세트 완료            │ │  ← 하단 고정 완료 버튼
│  └─────────────────────────────┘ │
└─────────────────────────────────┘
```

## 🔧 기술적 구현

### 💾 데이터 모델
```swift
// Domain Entity
struct WorkoutRecord {
    let id: UUID
    let exerciseName: String
    let category: WorkoutCategory
    let date: Date
    let sets: [WorkoutSet]
    
    var totalVolume: Double {
        sets.reduce(0) { $0 + $1.volume }
    }
    
    var isCompleted: Bool {
        !sets.isEmpty && sets.allSatisfy { $0.isCompleted }
    }
}

struct WorkoutSet {
    let id: UUID
    let weight: Double
    let reps: Int
    let isCompleted: Bool
    let completedAt: Date?
    
    var volume: Double {
        weight * Double(reps)
    }
}

enum WorkoutCategory: String, CaseIterable {
    case upperBody = "상체"
    case lowerBody = "하체"
    case fullBody = "전신"
    case cardio = "유산소"
}
```

### 🏗️ Repository Protocol
```swift
protocol WorkoutRepository {
    func addWorkout(_ workout: WorkoutRecord) async throws
    func updateWorkout(_ workout: WorkoutRecord) async throws
    func deleteWorkout(id: UUID) async throws
    func getWorkouts(for date: Date) async throws -> [WorkoutRecord]
    func getRecentExercises(limit: Int) async throws -> [String]
    func getPersonalRecord(for exercise: String) async throws -> WorkoutSet?
}
```

### 🎮 ViewModel
```swift
@MainActor
class WorkoutViewModel: ObservableObject {
    @Published var todayWorkouts: [WorkoutRecord] = []
    @Published var currentWorkout: WorkoutRecord?
    @Published var isAddingSet = false
    @Published var currentSetInput = SetInput()
    
    private let workoutService: WorkoutService
    
    func addNewWorkout(exerciseName: String) {
        // 빠른 운동 추가 로직
    }
    
    func addSet(to workoutId: UUID, weight: Double, reps: Int) {
        // 빠른 세트 추가 로직
    }
    
    func completeSet() {
        // 세트 완료 처리 및 PR 체크
    }
}

struct SetInput {
    var weight: Double = 0.0
    var reps: Int = 0
    var suggestedWeight: Double = 0.0
    var suggestedReps: Int = 0
}
```

## 🧪 테스트 시나리오

### ⚡ Unit Tests
```swift
class WorkoutRecordTests: XCTestCase {
    func testVolumeCalculation() {
        // 볼륨 계산 정확성 테스트
        let set = WorkoutSet(weight: 80, reps: 10)
        XCTAssertEqual(set.volume, 800.0)
    }
    
    func testPersonalRecordDetection() {
        // PR 감지 로직 테스트
    }
    
    func testAutoSuggestion() {
        // 자동 제안 로직 테스트
    }
}
```

### 📱 UI Tests
```swift
class WorkoutRecordUITests: XCTestCase {
    func testQuickWorkoutEntry() {
        app.buttons["addWorkoutButton"].tap()
        app.textFields["exerciseNameInput"].typeText("벤치프레스")
        app.buttons["confirmExercise"].tap()
        
        // 3탭 이내 세트 기록 완료 테스트
        app.buttons["weightPlus"].tap() // 무게 조정
        app.buttons["repsPlus"].tap()   // 횟수 조정  
        app.buttons["completeSetButton"].tap() // 완료
        
        XCTAssertTrue(app.staticTexts["세트 완료됨"].exists)
    }
    
    func testPersonalRecordCelebration() {
        // PR 달성 시 축하 애니메이션 테스트
    }
}
```

### 🎯 수동 테스트 체크리스트
- [ ] 한 손으로 전체 세트 기록 완료 가능
- [ ] 세트당 평균 기록 시간 10초 이내
- [ ] 땀에 젖은 손으로도 정확한 터치 가능
- [ ] 밝은 헬스장에서 화면 가독성 양호
- [ ] PR 달성 시 즉시 피드백 제공
- [ ] 실수 입력 시 쉬운 수정 가능

## 📊 성능 요구사항

### ⚡ 응답 시간
- **세트 추가**: 500ms 이내
- **운동 선택**: 300ms 이내  
- **자동완성**: 100ms 이내
- **볼륨 계산**: 즉시 (동기)

### 💾 데이터 처리
- **즉시 저장**: 세트 완료 시 Core Data에 즉시 저장
- **오프라인 우선**: 네트워크 없이 모든 기능 동작
- **메모리 효율**: 당일 데이터만 메모리에 유지

## ✅ 완료 기준 (DoD)

### 🎯 기능 완성도
- [ ] 운동 추가/선택 3탭 이내 완료
- [ ] 세트 기록 10초 이내 완료  
- [ ] PR 감지 및 축하 피드백 동작
- [ ] 볼륨 자동 계산 및 표시
- [ ] 오늘의 운동 목록 실시간 업데이트

### 🧪 품질 검증
- [ ] Unit 테스트 100% 통과
- [ ] UI 테스트 핵심 시나리오 통과
- [ ] 수동 테스트 체크리스트 100% 완료
- [ ] VoiceOver 접근성 100% 지원
- [ ] 성능 요구사항 100% 충족

### 📱 사용자 검증
- [ ] 실제 헬스장에서 10분 이상 사용 테스트
- [ ] 한 손 조작으로 전체 운동 기록 완료
- [ ] 평균 세트 기록 시간 10초 이내 달성
- [ ] 사용 중 앱 크래시 0건
- [ ] 데이터 손실 0건

이 기능은 FitnessTracker 앱의 핵심이며, 사용자가 운동에 집중하면서도 정확하고 빠르게 기록할 수 있는 최적의 경험을 제공합니다.