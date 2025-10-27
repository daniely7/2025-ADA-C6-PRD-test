# 🧪 FitnessTracker 테스트 전략 (Test.md)

이 문서는 FitnessTracker 앱의 품질 보증을 위한 종합적인 테스트 전략을 정의합니다. 모든 테스트는 반드시 통과해야 하며, 실패 시 원인 분석과 수정이 완료되어야 합니다.

## 🎯 테스트 목표

### 📊 품질 지표 목표
- **Unit 테스트 커버리지**: 80% 이상
- **UI 테스트 통과율**: 100%
- **수동 테스트 통과율**: 100%
- **성능 기준 충족율**: 100%
- **접근성 준수율**: 100%

### 🏆 사용자 경험 목표
- **세트 기록 완료 시간**: 평균 10초 이내
- **앱 안정성**: 크래시율 0.1% 이하
- **데이터 무결성**: 데이터 손실 0건
- **반응성**: 모든 액션 500ms 이내 응답

---

## 🔬 Unit Tests (단위 테스트)

### 🏗️ Domain Layer Tests

#### 💪 WorkoutRecord Tests
```swift
// Tests/Domain/WorkoutRecordTests.swift
class WorkoutRecordTests: XCTestCase {
    
    func testTotalVolumeCalculation() {
        // Given: 여러 세트가 있는 운동 기록
        let sets = [
            WorkoutSet(id: UUID(), weight: 80, reps: 10, isCompleted: true, completedAt: Date()),
            WorkoutSet(id: UUID(), weight: 80, reps: 8, isCompleted: true, completedAt: Date()),
            WorkoutSet(id: UUID(), weight: 75, reps: 8, isCompleted: true, completedAt: Date())
        ]
        let workout = WorkoutRecord(
            id: UUID(),
            exerciseName: "벤치프레스",
            category: .upperBody,
            date: Date(),
            sets: sets,
            createdAt: Date()
        )
        
        // When: 총 볼륨 계산
        let totalVolume = workout.totalVolume
        
        // Then: 정확한 볼륨 값 검증
        XCTAssertEqual(totalVolume, 2440.0) // (80*10) + (80*8) + (75*8) = 2440
    }
    
    func testWorkoutCompletion() {
        // 운동 완료 상태 확인 테스트
        let completedSets = [
            WorkoutSet(id: UUID(), weight: 100, reps: 5, isCompleted: true, completedAt: Date())
        ]
        let workout = WorkoutRecord(
            id: UUID(),
            exerciseName: "스쿼트",
            category: .lowerBody,
            date: Date(),
            sets: completedSets,
            createdAt: Date()
        )
        
        XCTAssertTrue(workout.isCompleted)
    }
    
    func testAverageWeightCalculation() {
        // 평균 무게 계산 테스트
    }
    
    func testEmptyWorkoutHandling() {
        // 빈 운동 기록 처리 테스트
    }
}
```

#### 🎯 WorkoutSet Tests
```swift
// Tests/Domain/WorkoutSetTests.swift
class WorkoutSetTests: XCTestCase {
    
    func testVolumeCalculation() {
        // Given: 특정 무게와 횟수의 세트
        let set = WorkoutSet(
            id: UUID(),
            weight: 120.0,
            reps: 5,
            isCompleted: true,
            completedAt: Date()
        )
        
        // When: 볼륨 계산
        let volume = set.volume
        
        // Then: 정확한 계산 확인
        XCTAssertEqual(volume, 600.0)
    }
    
    func testPersonalRecordDetection() {
        // PR 감지 로직 테스트 (추후 구현)
    }
    
    func testSetValidation() {
        // 유효하지 않은 세트 데이터 처리 테스트
    }
}
```

### 🔧 Service Layer Tests

#### 💼 WorkoutService Tests  
```swift
// Tests/Domain/WorkoutServiceTests.swift
class WorkoutServiceTests: XCTestCase {
    var workoutService: WorkoutService!
    var mockRepository: MockWorkoutRepository!
    
    override func setUp() {
        super.setUp()
        mockRepository = MockWorkoutRepository()
        workoutService = WorkoutServiceImpl(repository: mockRepository)
    }
    
    override func tearDown() {
        workoutService = nil
        mockRepository = nil
        super.tearDown()
    }
    
    func testAddWorkout() async throws {
        // Given: 새 운동 정보
        let exerciseName = "데드리프트"
        let expectedCategory = WorkoutCategory.lowerBody
        
        // When: 운동 추가
        let workout = try await workoutService.addWorkout(
            exerciseName: exerciseName,
            category: expectedCategory
        )
        
        // Then: 올바른 운동 생성 확인
        XCTAssertEqual(workout.exerciseName, exerciseName)
        XCTAssertEqual(workout.category, expectedCategory)
        XCTAssertTrue(workout.sets.isEmpty)
        XCTAssertEqual(mockRepository.workouts.count, 1)
    }
    
    func testAddSet() async throws {
        // Given: 기존 운동에 세트 추가
        let workout = try await workoutService.addWorkout(exerciseName: "벤치프레스", category: .upperBody)
        
        // When: 세트 추가
        try await workoutService.addSet(to: workout.id, weight: 80.0, reps: 10)
        
        // Then: 세트가 올바르게 추가되었는지 확인
        let updatedWorkouts = try await workoutService.getTodayWorkouts()
        let updatedWorkout = updatedWorkouts.first { $0.id == workout.id }
        
        XCTAssertNotNil(updatedWorkout)
        XCTAssertEqual(updatedWorkout?.sets.count, 1)
        XCTAssertEqual(updatedWorkout?.sets.first?.weight, 80.0)
        XCTAssertEqual(updatedWorkout?.sets.first?.reps, 10)
    }
    
    func testPersonalRecordDetection() async throws {
        // Given: 기존 기록이 있는 운동
        let exerciseName = "벤치프레스"
        let existingRecord = WorkoutSet(
            id: UUID(),
            weight: 100.0,
            reps: 5,
            isCompleted: true,
            completedAt: Date()
        )
        mockRepository.personalRecords[exerciseName] = existingRecord
        
        // When: 더 높은 기록 시도
        let isNewRecord = try await workoutService.detectPersonalRecord(
            for: exerciseName,
            weight: 105.0,
            reps: 5
        )
        
        // Then: PR 감지 확인
        XCTAssertTrue(isNewRecord)
    }
    
    func testInvalidWeightHandling() async {
        // 유효하지 않은 무게 입력 처리 테스트
        do {
            try await workoutService.addSet(to: UUID(), weight: -10.0, reps: 5)
            XCTFail("음수 무게는 오류를 발생시켜야 함")
        } catch WorkoutError.invalidWeight {
            // 예상된 오류
        } catch {
            XCTFail("예상하지 못한 오류: \(error)")
        }
    }
    
    func testWeeklyProgressCalculation() async throws {
        // 주간 진행률 계산 테스트
    }
    
    func testSetSuggestion() async throws {
        // 세트 제안 로직 테스트
    }
}
```

### 💾 Data Layer Tests

#### 🗄️ CoreDataRepository Tests
```swift
// Tests/Data/CoreDataWorkoutRepositoryTests.swift
class CoreDataWorkoutRepositoryTests: XCTestCase {
    var repository: CoreDataWorkoutRepository!
    var testContext: NSManagedObjectContext!
    
    override func setUp() {
        super.setUp()
        testContext = PersistenceController.preview.container.viewContext
        repository = CoreDataWorkoutRepository(context: testContext)
    }
    
    func testSaveAndRetrieveWorkout() async throws {
        // Given: 새 운동 기록
        let workout = WorkoutRecord(
            id: UUID(),
            exerciseName: "스쿼트",
            category: .lowerBody,
            date: Date(),
            sets: [],
            createdAt: Date()
        )
        
        // When: 저장 후 조회
        try await repository.addWorkout(workout)
        let retrieved = try await repository.getWorkouts(for: Date())
        
        // Then: 데이터 일치 확인
        XCTAssertEqual(retrieved.count, 1)
        XCTAssertEqual(retrieved.first?.exerciseName, "스쿼트")
        XCTAssertEqual(retrieved.first?.category, .lowerBody)
    }
    
    func testDeleteWorkout() async throws {
        // 운동 삭제 테스트
    }
    
    func testDateRangeQuery() async throws {
        // 날짜 범위 조회 테스트
    }
    
    func testConcurrentAccess() async throws {
        // 동시 접근 안전성 테스트
    }
}
```

---

## 📱 UI Tests (사용자 인터페이스 테스트)

### 🚀 핵심 사용자 시나리오 Tests

#### 💪 WorkoutRecordUITests
```swift
// UITests/WorkoutRecordUITests.swift
class WorkoutRecordUITests: XCTestCase {
    var app: XCUIApplication!
    
    override func setUp() {
        super.setUp()
        continueAfterFailure = false
        app = XCUIApplication()
        app.launch()
    }
    
    func testQuickWorkoutEntry() {
        // **시나리오**: 헬스장에서 빠른 운동 기록
        // **목표**: 10초 이내 세트 기록 완료
        
        let startTime = Date()
        
        // 1. 운동 추가 버튼 터치
        let addButton = app.buttons["addWorkoutButton"]
        XCTAssertTrue(addButton.waitForExistence(timeout: 3))
        addButton.tap()
        
        // 2. 운동명 입력
        let exerciseField = app.textFields["exerciseNameInput"]
        XCTAssertTrue(exerciseField.waitForExistence(timeout: 2))
        exerciseField.tap()
        exerciseField.typeText("벤치프레스")
        
        app.buttons["confirmExercise"].tap()
        
        // 3. 첫 세트 기록 (3탭 이내)
        let weightPlus = app.buttons["weightPlus"]
        let repsPlus = app.buttons["repsPlus"]
        let completeButton = app.buttons["completeSetButton"]
        
        XCTAssertTrue(weightPlus.waitForExistence(timeout: 2))
        
        // 무게 조정 (80kg)
        for _ in 1...8 { weightPlus.tap() } // 0 -> 80kg (2.5kg씩)
        
        // 횟수 조정 (10회)
        for _ in 1...10 { repsPlus.tap() } // 0 -> 10회
        
        // 완료
        completeButton.tap()
        
        // 4. 결과 확인
        let completionMessage = app.staticTexts["세트 완료됨"]
        XCTAssertTrue(completionMessage.waitForExistence(timeout: 2))
        
        // 5. 시간 검증
        let elapsedTime = Date().timeIntervalSince(startTime)
        XCTAssertLessThan(elapsedTime, 10.0, "세트 기록이 10초를 초과했습니다")
    }
    
    func testPersonalRecordCelebration() {
        // **시나리오**: PR 달성 시 축하 메시지 표시
        
        // 기존 기록보다 높은 무게로 세트 기록
        // ... 구현
        
        // PR 축하 메시지 확인
        let prMessage = app.alerts["새로운 개인 기록!"]
        XCTAssertTrue(prMessage.waitForExistence(timeout: 3))
        
        let goldBadge = app.images["prBadge"]
        XCTAssertTrue(goldBadge.exists)
    }
    
    func testWorkoutListNavigation() {
        // **시나리오**: 운동 목록 탐색 및 상세 조회
        
        let workoutList = app.scrollViews["workoutList"]
        XCTAssertTrue(workoutList.exists)
        
        // 운동 항목 탭
        let firstWorkout = app.buttons["workoutRow"].firstMatch
        if firstWorkout.exists {
            firstWorkout.tap()
            
            // 상세 화면 진입 확인
            XCTAssertTrue(app.navigationBars["운동 상세"].waitForExistence(timeout: 2))
        }
    }
    
    func testSwipeToDelete() {
        // **시나리오**: 스와이프로 세트 삭제
        
        let firstSet = app.buttons["setChip"].firstMatch
        if firstSet.exists {
            firstSet.swipeLeft()
            
            let deleteButton = app.buttons["deleteSetButton"]
            XCTAssertTrue(deleteButton.waitForExistence(timeout: 1))
            deleteButton.tap()
            
            // 삭제 확인
            XCTAssertFalse(firstSet.exists)
        }
    }
    
    func testOneHandedOperation() {
        // **시나리오**: 한 손으로 전체 기능 조작 가능
        
        // 화면 하단 1/3 영역의 버튼들만 사용하여 전체 운동 기록 완료
        let screenHeight = app.frame.height
        let oneHandedZone = screenHeight * 0.67 // 하단 33%
        
        // 모든 주요 액션 버튼이 한 손 영역에 있는지 확인
        let addButton = app.buttons["addWorkoutButton"]
        let completeButton = app.buttons["completeSetButton"]
        
        XCTAssertLessThan(addButton.frame.minY, oneHandedZone)
        XCTAssertLessThan(completeButton.frame.minY, oneHandedZone)
    }
}
```

### ♿ 접근성 UI Tests

#### 🔊 VoiceOverUITests
```swift
// UITests/VoiceOverUITests.swift
class VoiceOverUITests: XCTestCase {
    var app: XCUIApplication!
    
    override func setUp() {
        super.setUp()
        app = XCUIApplication()
        app.accessibilityActivationPoint = CGPoint(x: 0, y: 0)
        app.launch()
    }
    
    func testVoiceOverWorkoutEntry() {
        // VoiceOver 사용자의 운동 기록 시나리오
        
        let addButton = app.buttons["addWorkoutButton"]
        XCTAssertEqual(addButton.label, "운동 추가 버튼")
        XCTAssertNotNil(addButton.value)
        
        // 모든 접근성 레이블이 한국어로 제공되는지 확인
        let weightInput = app.textFields["weightInput"]
        XCTAssertEqual(weightInput.label, "무게 입력 필드")
        
        let repsInput = app.textFields["repsInput"] 
        XCTAssertEqual(repsInput.label, "횟수 입력 필드")
    }
    
    func testAccessibilityIdentifiers() {
        // 모든 중요한 UI 요소에 접근성 ID가 있는지 확인
        
        let requiredIds = [
            "addWorkoutButton",
            "workoutList", 
            "workoutRow",
            "setInput",
            "weightInput",
            "repsInput",
            "completeSetButton",
            "deleteSetButton",
            "progressChart",
            "statisticsView"
        ]
        
        for id in requiredIds {
            let element = app.descendants(matching: .any)[id]
            XCTAssertTrue(element.exists, "\(id) 접근성 식별자가 없습니다")
        }
    }
    
    func testDynamicTypeSupport() {
        // Dynamic Type 지원 테스트
        // 텍스트 크기 변경 시 레이아웃이 깨지지 않는지 확인
    }
}
```

---

## 🖐️ Manual Tests (수동 테스트)

### 🏋️‍♂️ 실제 사용 환경 테스트

#### 📋 헬스장 실전 테스트 체크리스트
```markdown
**테스트 환경**: 실제 헬스장, 운동 중
**테스트 기간**: 최소 30분 운동 세션
**테스터**: 실제 헬스장 이용자

□ **빠른 기록 테스트**
  □ 세트 간 휴식 시간(30초-1분) 내에 기록 완료
  □ 한 손으로 물병 들고 다른 손으로 조작 가능
  □ 평균 기록 시간 10초 이내 달성
  □ 연속 3세트 기록 시 피로감으로 인한 실수 없음

□ **물리적 조건 테스트**  
  □ 땀에 젖은 손으로 터치 정확도 유지
  □ 운동용 장갑 착용 시 정상 조작
  □ 밝은 헬스장 조명에서 화면 가독성 양호
  □ 운동 중 진동이 있는 환경에서 안정적 조작

□ **사용성 테스트**
  □ 무게 조정이 직관적이고 빠름 (2.5kg 단위)
  □ 이전 세트 정보가 명확하게 표시됨
  □ 실수 입력 시 빠른 수정 가능
  □ 운동 전환이 2탭 이내로 완료

□ **동기부여 테스트**
  □ 세트 완료 시 즉시 성취감 제공
  □ 총 볼륨이 실시간으로 업데이트됨
  □ PR 달성 시 즉시 인지 가능
  □ 진행 상황이 명확하게 표시됨
```

#### 🔄 연속 사용 테스트
```markdown
**테스트 목표**: 1주일 연속 사용 시 안정성 및 일관성

□ **데이터 무결성**
  □ 7일간 모든 기록이 정확히 저장됨
  □ 앱 재시작 후에도 데이터 유지
  □ 메모리 부족 상황에서도 데이터 손실 없음
  □ 백그라운드 전환 후 입력 중인 데이터 유지

□ **성능 안정성**
  □ 1주일 사용 후에도 앱 시작 시간 3초 이내
  □ 메모리 사용량 증가 없음
  □ 배터리 소모 정상 수준 유지
  □ 크래시 발생 없음

□ **사용 패턴 적응**
  □ 자주 사용하는 운동이 우선 표시됨
  □ 개인의 평균 무게가 기본값으로 설정됨
  □ 운동 패턴에 따른 스마트 제안 제공
  □ 진행률 계산이 정확함
```

### 🧪 스트레스 테스트

#### 📊 대용량 데이터 테스트
```markdown
**테스트 시나리오**: 6개월간의 운동 기록 시뮬레이션

□ **데이터 로딩 성능**
  □ 1000개 운동 기록 로딩 시간 3초 이내
  □ 스크롤 성능 저하 없음
  □ 검색 기능 응답 시간 500ms 이내
  □ 통계 계산 시간 2초 이내

□ **메모리 관리**
  □ 대용량 데이터에서 메모리 누수 없음
  □ 백그라운드 정리 정상 동작
  □ 오래된 데이터 아카이빙 기능 동작
  □ 캐시 관리 효율성 확인
```

#### ⚠️ 에지 케이스 테스트
```markdown
**테스트 목표**: 예외 상황에서의 앱 안정성

□ **비정상 입력 처리**
  □ 음수 무게 입력 시 적절한 오류 메시지
  □ 과도하게 큰 수치 입력 시 제한 적용
  □ 빈 문자열 운동명 입력 방지
  □ 특수문자 입력 시 안정적 처리

□ **시스템 리소스 부족**
  □ 메모리 부족 시 우아한 처리
  □ 저장 공간 부족 시 적절한 알림
  □ 네트워크 없는 환경에서 정상 동작
  □ 배터리 절약 모드에서 성능 유지

□ **동시성 처리**
  □ 빠른 연속 입력 시 데이터 일관성 유지
  □ 앱 전환 중 데이터 입력 시 안전 처리
  □ 여러 세트 동시 완료 시 정확한 처리
  □ 백그라운드 저장 중 앱 종료 시 데이터 보호
```

---

## 🚀 Performance Tests (성능 테스트)

### ⚡ 응답 시간 테스트

#### 📊 측정 기준 및 도구
```swift
// Tests/Performance/PerformanceTests.swift
class PerformanceTests: XCTestCase {
    
    func testSetRecordingSpeed() {
        // 세트 기록 완료 시간 측정
        measure(metrics: [XCTClockMetric()]) {
            // 세트 기록 프로세스 실행
            recordWorkoutSet(weight: 80.0, reps: 10)
        }
        // 기준: 500ms 이내
    }
    
    func testAppLaunchTime() {
        // 앱 시작 시간 측정
        measure(metrics: [XCTApplicationLaunchMetric()]) {
            XCUIApplication().launch()
        }
        // 기준: 3초 이내
    }
    
    func testScrollPerformance() {
        // 운동 목록 스크롤 성능 측정
        measure(metrics: [XCTOSSignpostMetric.scrollingAndDecelerationMetric]) {
            // 대용량 목록 스크롤 테스트
        }
    }
    
    func testStatisticsCalculation() {
        // 통계 계산 성능 측정
        measure {
            calculateWeeklyProgress(for: generateLargeDataset())
        }
        // 기준: 2초 이내
    }
}
```

#### 🎯 성능 목표
```markdown
**필수 성능 기준 (모두 충족 필요)**

□ **사용자 응답성**
  □ 세트 기록 완료: 500ms 이내
  □ 운동 추가: 300ms 이내  
  □ 화면 전환: 500ms 이내
  □ 데이터 저장: 200ms 이내

□ **앱 실행 성능**
  □ 콜드 스타트: 3초 이내
  □ 웜 스타트: 1초 이내
  □ 백그라운드 복원: 500ms 이내
  □ 메모리 사용량: 50MB 이하

□ **데이터 처리 성능**
  □ 1000개 기록 로딩: 2초 이내
  □ 통계 계산: 2초 이내
  □ 검색 결과: 500ms 이내
  □ 데이터베이스 쿼리: 100ms 이내
```

---

## ✅ DoD (Definition of Done) - 완료 기준

### 🎯 기능 완성도 기준

#### 💪 핵심 기능 DoD
```markdown
**운동 기록 기능**
□ 운동 추가가 3탭 이내로 완료됨
□ 세트 기록이 10초 이내로 완료됨
□ PR 감지 및 축하 피드백이 즉시 제공됨
□ 볼륨 자동 계산이 정확히 동작함
□ 오늘의 운동 목록이 실시간 업데이트됨

**데이터 무결성**  
□ 모든 입력 데이터가 검증됨
□ 잘못된 입력 시 명확한 오류 메시지 제공
□ 데이터 손실 없이 안정적 저장
□ 앱 재시작 후 데이터 유지
□ 백그라운드 전환 시 입력 데이터 보존
```

### 🧪 품질 보증 기준

#### 📊 테스트 통과 기준
```markdown
**자동화 테스트**
□ Unit 테스트 80% 이상 커버리지
□ Unit 테스트 100% 통과
□ UI 테스트 핵심 시나리오 100% 통과
□ 성능 테스트 모든 기준 충족
□ 메모리 누수 테스트 통과

**수동 테스트**
□ 헬스장 실전 테스트 체크리스트 100% 완료
□ 한 손 조작 테스트 완료
□ VoiceOver 접근성 테스트 100% 통과
□ 다양한 화면 크기에서 레이아웃 검증
□ 극한 상황 테스트 완료
```

#### ♿ 접근성 준수 기준
```markdown
**접근성 필수 요구사항**
□ 모든 UI 요소에 접근성 레이블 제공 (한국어)
□ VoiceOver 100% 호환성
□ Dynamic Type 지원 (최대 3단계 크기 증가)
□ 고대비 모드 지원
□ 최소 터치 영역 44x44pt 보장
□ 키보드 네비게이션 지원
```

### 📱 사용자 검증 기준

#### 🏋️‍♂️ 실제 사용성 검증
```markdown
**실전 사용 테스트**
□ 실제 헬스장에서 30분 이상 연속 사용 완료
□ 한 손으로 전체 운동 세션 기록 완료
□ 평균 세트 기록 시간 10초 이내 달성
□ 사용 중 앱 크래시 0건
□ 데이터 손실 사고 0건

**사용자 만족도**
□ 직관적인 인터페이스로 학습 곡선 최소화
□ 빠른 기록으로 운동 집중도 방해 없음
□ 실시간 피드백으로 즉시 성취감 제공
□ 진행률 확인으로 지속적 동기부여
□ 안정적인 동작으로 신뢰성 확보
```

---

## 🚨 테스트 실패 시 대응 절차

### ⚠️ 심각도별 대응

#### 🔴 Critical (심각) - 즉시 수정 필요
- 앱 크래시 또는 데이터 손실
- 핵심 기능 완전 불능
- 보안 취약점 발견

#### 🟡 High (높음) - 24시간 내 수정
- 성능 기준 미달성
- 접근성 기본 요구사항 미충족
- 사용자 경험 저해 요소

#### 🟢 Medium (보통) - 다음 릴리스에서 수정
- 사소한 UI 결함
- 성능 최적화 여지
- 편의성 개선 사항

### 📝 테스트 결과 보고

#### 📊 테스트 결과 문서화
```markdown
**테스트 결과 요약**
- 실행 일시: [날짜/시간]
- 테스트 환경: [디바이스/iOS 버전]
- 전체 테스트 수: [숫자]
- 통과 테스트 수: [숫자]  
- 실패 테스트 수: [숫자]
- 성공률: [백분율]

**실패 테스트 상세**
- 테스트명: [구체적 이름]
- 실패 원인: [기술적 원인]
- 재현 방법: [단계별 설명]
- 수정 방안: [구체적 해결책]
- 예상 수정 시간: [시간]
```

이 포괄적인 테스트 전략을 통해 FitnessTracker 앱의 품질과 사용자 만족도를 보장할 수 있습니다. 모든 테스트는 실제 사용 환경을 고려하여 설계되었으며, 헬스장에서의 실전 사용성을 최우선으로 검증합니다.