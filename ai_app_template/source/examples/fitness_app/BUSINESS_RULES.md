# 🏋️‍♂️ 피트니스 앱 비즈니스 규칙 (BUSINESS_RULES.md)

## 🎯 핵심 비즈니스 로직

### 💪 운동 분류 체계
```swift
enum WorkoutCategory: String, CaseIterable {
    case upperBody = "상체"      // 벤치프레스, 숄더프레스, 랫풀다운 등
    case lowerBody = "하체"      // 스쿼트, 데드리프트, 레그프레스 등  
    case fullBody = "전신"       // 버피, 클린앤저크, 스내치 등
    case cardio = "유산소"       // 러닝머신, 사이클, 로잉머신 등
}
```

### 📊 볼륨 계산 규칙
1. **세트 볼륨** = 무게(kg) × 횟수(reps)
2. **운동 볼륨** = 모든 세트 볼륨의 합
3. **일일 총 볼륨** = 모든 운동 볼륨의 합
4. **주간 볼륨** = 7일간 총 볼륨의 합

### 🏆 개인 기록(PR) 정의
- **1RM (One Rep Max)**: 해당 운동의 최고 무게 × 1회
- **볼륨 PR**: 하루 단일 운동의 최고 총 볼륨
- **강도 PR**: 특정 무게로 달성한 최고 횟수

## 📋 데이터 입력 규칙

### ✅ 유효성 검사 (Validation Rules)
```swift
struct WorkoutValidation {
    static let weightRange = 0.0...500.0      // kg 단위
    static let repsRange = 1...100             // 횟수
    static let setLimit = 1...20               // 세트 수 제한
    static let workoutNameMaxLength = 50       // 운동명 최대 길이
    static let notesMaxLength = 200            // 메모 최대 길이
}
```

### 🔄 자동 입력 로직
1. **이전 세트 복사**: 새 세트 추가 시 바로 이전 세트의 무게/횟수를 기본값으로 설정
2. **지난주 동일 운동**: 같은 요일, 같은 운동의 지난주 기록을 참조 제안
3. **점진적 과부하**: 지난 세트보다 무게 +2.5kg 또는 횟수 +1 제안

### 📝 필수/선택 입력 필드
- **필수**: 운동명, 무게, 횟수
- **선택**: 세트 순서(자동 생성), 메모, 휴식 시간
- **자동**: 날짜/시간, 볼륨 계산, 운동 카테고리 추론

## ⏰ 시간 관리 규칙

### 📅 운동 세션 정의
```swift
struct WorkoutSession {
    let startTime: Date
    let endTime: Date?
    let exercises: [Exercise]
    
    // 세션 지속시간 계산
    var duration: TimeInterval {
        guard let end = endTime else { return 0 }
        return end.timeIntervalSince(startTime)
    }
}
```

### 🕐 자동 세션 관리
1. **세션 시작**: 첫 번째 세트 기록 시 자동 시작
2. **세션 종료**: 마지막 기록 후 30분 경과 시 자동 종료
3. **중단 감지**: 15분 이상 기록이 없으면 "운동을 계속하시겠습니까?" 알림

## 📈 진행률 계산 규칙

### 📊 주간 비교 로직
```swift
extension ProgressCalculator {
    func weeklyComparison() -> WeeklyProgress {
        let thisWeek = getCurrentWeekVolume()
        let lastWeek = getLastWeekVolume()
        let change = ((thisWeek - lastWeek) / lastWeek) * 100
        
        return WeeklyProgress(
            current: thisWeek,
            previous: lastWeek,
            changePercentage: change,
            trend: change > 5 ? .improving : change < -5 ? .declining : .stable
        )
    }
}
```

### 🎯 목표 설정 및 추적
1. **주간 운동 일수 목표**: 기본 3일, 사용자 조정 가능 (1-7일)
2. **주간 볼륨 목표**: 지난 4주 평균의 105%로 자동 설정
3. **개인 기록 갱신 목표**: 월 1회 이상 PR 달성

## 🎮 사용자 경험 규칙

### ⚡ 빠른 입력 최적화
```swift
struct QuickInput {
    // 무게 입력: 2.5kg 단위로 조정 (한국 헬스장 표준)
    static let weightIncrement = 2.5
    
    // 자주 사용하는 무게들을 우선 표시
    static let commonWeights = [20, 40, 60, 80, 100, 120] // kg
    
    // 횟수 자주 사용 범위
    static let commonReps = [8, 10, 12, 15]
}
```

### 🔄 스마트 기본값
1. **운동명 자동완성**: 기존 운동 목록에서 타이핑 기반 필터링
2. **무게 제안**: 같은 운동의 최근 평균 무게 ±10% 범위
3. **세트 수 제안**: 해당 운동의 평균 세트 수 기준

### 🎨 시각적 피드백 규칙
```swift
enum SetStatus {
    case planned        // 회색 - 계획된 세트
    case inProgress     // 노란색 - 현재 진행 중
    case completed      // 초록색 - 완료된 세트
    case personalRecord // 금색 - 개인 기록 달성
}
```

## 🔒 데이터 보안 및 백업

### 💾 로컬 저장 규칙
1. **Core Data 우선**: 모든 데이터는 먼저 로컬에 저장
2. **즉시 저장**: 각 세트 완료 시 즉시 데이터베이스에 저장
3. **임시 데이터**: 입력 중인 데이터는 메모리에만 유지, 완료 시 영구 저장

### 🔄 데이터 무결성
```swift
class DataIntegrityManager {
    // 중복 세트 방지
    func validateSet(_ set: WorkoutSet) -> Bool {
        return !hasDuplicateSet(set) && isValidWeight(set.weight) && isValidReps(set.reps)
    }
    
    // 데이터 일관성 검사
    func checkDataConsistency() {
        validateWorkoutVolumes()
        validateDateRanges()
        validatePersonalRecords()
    }
}
```

## 📱 접근성 및 사용성 규칙

### ♿ 접근성 준수
1. **VoiceOver 호환**: 모든 UI 요소에 적절한 accessibility label 제공
2. **큰 터치 영역**: 최소 44x44pt 터치 영역 보장
3. **고대비 지원**: Dynamic Type 및 고대비 모드 지원

### 🤏 한 손 사용 최적화
```swift
struct OneHandedDesign {
    // 주요 액션은 화면 하단 1/3에 배치
    static let primaryActionZone = 0.67...1.0  // 화면 하단 33%
    
    // 중요하지 않은 정보는 상단에 배치
    static let infoDisplayZone = 0.0...0.33    // 화면 상단 33%
}
```

## 📊 성능 최적화 규칙

### ⚡ 응답 시간 목표
- **세트 기록**: 500ms 이내 완료
- **운동 전환**: 1초 이내 완료  
- **통계 로딩**: 2초 이내 완료
- **앱 시작**: 3초 이내 메인 화면 표시

### 💾 메모리 관리
```swift
class PerformanceManager {
    // 최근 30일 데이터만 메모리에 유지
    private let memoryDataRange = 30 // days
    
    // 통계 계산 결과 캐싱 (1시간)
    private let statsCacheExpiry = 3600 // seconds
    
    // 이미지나 불필요한 데이터 최소화
    private let maxCacheSize = 50 * 1024 * 1024 // 50MB
}
```

## 🎯 비즈니스 의사결정 우선순위

### 🥇 사용자 경험 우선
1. **빠른 입력** > 정확한 분석
2. **직관적 UI** > 고급 기능  
3. **안정성** > 최신 기술
4. **오프라인 동작** > 클라우드 동기화

### 📈 기능 개발 우선순위
```swift
enum FeaturePriority: Int {
    case critical = 1    // 운동 기록, 기본 통계
    case important = 2   // 진행률 추적, PR 감지
    case nice_to_have = 3 // 고급 분석, 커스터마이징
    case future = 4      // 소셜 기능, AI 추천
}
```

## 🚫 제외되는 기능들

### ❌ 스코프 외 기능
- **소셜 네트워킹**: 친구 추가, 경쟁, 공유 기능
- **복잡한 프로그램**: 전문 트레이닝 프로그램 제공
- **영양 관리**: 식단이나 칼로리 계산 기능
- **외부 기기 연동**: 스마트워치나 헬스 디바이스 연결

### ⚠️ 피해야 할 패턴
- 과도한 알림이나 푸시
- 복잡한 설정 메뉴
- 여러 단계의 확인 과정
- 불필요한 튜토리얼이나 온보딩

이러한 비즈니스 규칙들은 FitnessTracker 앱이 사용자의 핵심 니즈인 "빠른 기록, 명확한 진행률, 지속 가능한 동기부여"에 집중할 수 있도록 가이드합니다.