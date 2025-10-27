# 💰 가계부 앱 비즈니스 규칙 (BUSINESS_RULES.md)

## 🎯 핵심 비즈니스 로직

### 💸 거래 유형 분류
```swift
enum TransactionType: String, CaseIterable {
    case expense = "지출"       // 돈이 나가는 모든 거래
    case income = "수입"        // 돈이 들어오는 모든 거래
    case transfer = "이체"      // 계좌간 이동 (추후 확장)
}
```

### 🏷️ 카테고리 체계
```swift
// 기본 지출 카테고리
enum ExpenseCategory: String, CaseIterable {
    case food = "식비"           // 🍔 식당, 배달, 간식, 음료
    case transport = "교통비"     // 🚌 지하철, 버스, 택시, 주유
    case shopping = "쇼핑"        // 🛍️ 의류, 화장품, 생활용품
    case cafe = "카페/디저트"     // ☕ 커피, 디저트, 베이커리
    case culture = "문화/여가"    // 🎬 영화, 공연, 여행, 취미
    case medical = "의료/건강"    // 🏥 병원, 약국, 헬스, 마사지
    case utility = "공과금"       // 💡 전기, 가스, 수도, 인터넷
    case education = "교육"       // 📚 학원, 도서, 강의, 자격증
    case other = "기타"          // 🏷️ 기타 지출
}

// 기본 수입 카테고리  
enum IncomeCategory: String, CaseIterable {
    case salary = "급여"         // 💰 월급, 상여금
    case sideJob = "부업"        // 💼 프리랜서, 알바
    case investment = "투자수익"  // 📈 주식, 펀드 수익
    case gift = "용돈/선물"      // 🎁 부모님 용돈, 축의금
    case other = "기타수입"      // 💎 기타 수입
}
```

### 💵 금액 처리 규칙
```swift
struct AmountHandling {
    // 한국 원화 기본 규칙
    static let currency = "KRW"
    static let currencySymbol = "₩"
    static let minimumAmount = 0
    static let maximumAmount = 1_000_000_000 // 10억원
    
    // 표시 형식
    static func formatAmount(_ amount: Int) -> String {
        let formatter = NumberFormatter()
        formatter.numberStyle = .decimal
        formatter.groupingSeparator = ","
        return "₩\(formatter.string(from: NSNumber(value: amount)) ?? "0")"
    }
    
    // 입력 편의 기능
    static let quickAmounts = [1000, 5000, 10000, 50000, 100000] // 빠른 입력용
}
```

## 📊 예산 관리 규칙

### 🎯 예산 설정 및 추적
```swift
struct BudgetRules {
    // 예산 기간
    enum BudgetPeriod: String, CaseIterable {
        case monthly = "월별"
        case weekly = "주별"     // 추후 확장
    }
    
    // 예산 알림 임계값
    static let warningThreshold = 0.8    // 80% 소진 시 경고
    static let dangerThreshold = 0.95    // 95% 소진 시 위험
    
    // 기본 예산 제안 (월별)
    static let defaultBudgets: [ExpenseCategory: Int] = [
        .food: 300_000,          // 식비 30만원
        .transport: 100_000,     // 교통비 10만원
        .shopping: 150_000,      // 쇼핑 15만원
        .cafe: 80_000,          // 카페 8만원
        .culture: 100_000,       // 문화/여가 10만원
        .utility: 150_000,       // 공과금 15만원
        .other: 100_000         // 기타 10만원
    ]
}
```

### 📈 예산 초과 처리 로직
```swift
extension BudgetManager {
    func checkBudgetStatus(for category: ExpenseCategory, amount: Int) -> BudgetStatus {
        let currentSpent = getCurrentMonthSpent(for: category)
        let budget = getBudget(for: category)
        let newTotal = currentSpent + amount
        let percentage = Double(newTotal) / Double(budget)
        
        switch percentage {
        case 0..<0.8:
            return .safe
        case 0.8..<0.95:
            return .warning(message: "\(category.rawValue) 예산의 \(Int(percentage*100))%를 사용했습니다")
        case 0.95..<1.0:
            return .danger(message: "\(category.rawValue) 예산이 거의 소진되었습니다")
        default:
            return .exceeded(message: "\(category.rawValue) 예산을 \(formatAmount(newTotal - budget)) 초과했습니다")
        }
    }
}
```

## ⚡ 빠른 입력 최적화 규칙

### 🔄 스마트 제안 로직
```swift
class SmartSuggestionEngine {
    // 시간 기반 카테고리 제안
    func suggestCategoryByTime() -> ExpenseCategory {
        let hour = Calendar.current.component(.hour, from: Date())
        switch hour {
        case 7...9:   return .food        // 아침시간 → 식비
        case 12...14: return .food        // 점심시간 → 식비
        case 18...20: return .food        // 저녁시간 → 식비
        case 9...17:  return .cafe        // 근무시간 → 카페
        default:      return .other       // 기타시간 → 기타
        }
    }
    
    // 금액 기반 카테고리 제안
    func suggestCategoryByAmount(_ amount: Int) -> [ExpenseCategory] {
        switch amount {
        case 1000...6000:   return [.cafe, .transport]           // 소액 → 카페/교통
        case 6000...15000:  return [.food, .cafe]               // 중소액 → 식비/카페
        case 15000...50000: return [.food, .shopping]           // 중액 → 식비/쇼핑
        case 50000...:      return [.shopping, .culture]        // 고액 → 쇼핑/문화
        default:           return [.other]
        }
    }
    
    // 최근 거래 패턴 기반 제안
    func suggestFromRecentPatterns(amount: Int) -> ExpenseCategory? {
        let recentSimilar = getRecentTransactions()
            .filter { abs($0.amount - amount) <= 1000 }  // ±1000원 범위
            .prefix(5)
        
        guard !recentSimilar.isEmpty else { return nil }
        
        // 가장 빈번한 카테고리 반환
        let categoryCount = Dictionary(grouping: recentSimilar) { $0.category }
        return categoryCount.max(by: { $0.value.count < $1.value.count })?.key
    }
}
```

### 📝 입력 템플릿 및 반복 거래
```swift
struct TransactionTemplate {
    let name: String           // "스타벅스 아메리카노"
    let amount: Int           // 4500
    let category: ExpenseCategory  // .cafe
    let note: String?         // "매일 아침 커피"
    let frequency: TemplateFrequency
}

enum TemplateFrequency {
    case daily              // 매일 (출근길 커피)
    case weekly             // 매주 (주말 마트)
    case monthly            // 매월 (헬스장 회비)
    case occasional         // 가끔 (자주 가는 식당)
}
```

## 📅 날짜 및 시간 관리 규칙

### 🕐 거래 시간 처리
```swift
struct TransactionTiming {
    // 기본 날짜는 오늘
    static var defaultDate: Date { Date() }
    
    // 허용 가능한 날짜 범위 (과거 1년, 미래 7일)
    static var allowedDateRange: ClosedRange<Date> {
        let calendar = Calendar.current
        let pastLimit = calendar.date(byAdding: .year, value: -1, to: Date())!
        let futureLimit = calendar.date(byAdding: .day, value: 7, to: Date())!
        return pastLimit...futureLimit
    }
    
    // 자정 넘어서 기록하는 경우 처리
    func adjustDateForLateEntry() -> Date {
        let now = Date()
        let hour = Calendar.current.component(.hour, from: now)
        
        // 새벽 2시 이전이면 전날 거래로 간주
        if hour < 2 {
            return Calendar.current.date(byAdding: .day, value: -1, to: now) ?? now
        }
        return now
    }
}
```

## 🔒 데이터 무결성 및 검증 규칙

### ✅ 입력 데이터 검증
```swift
struct TransactionValidator {
    // 필수 필드 검증
    static func validateRequired(_ transaction: Transaction) throws {
        guard transaction.amount > 0 else {
            throw ValidationError.invalidAmount("금액은 0보다 커야 합니다")
        }
        
        guard !transaction.category.rawValue.isEmpty else {
            throw ValidationError.missingCategory("카테고리를 선택해주세요")
        }
        
        guard transaction.date <= Date() || 
              transaction.date <= Calendar.current.date(byAdding: .day, value: 7, to: Date())! else {
            throw ValidationError.invalidDate("미래 7일 이후 날짜는 입력할 수 없습니다")
        }
    }
    
    // 비즈니스 규칙 검증
    static func validateBusinessRules(_ transaction: Transaction) throws {
        // 일일 최대 거래 금액 제한 (1억원)
        guard transaction.amount <= 100_000_000 else {
            throw ValidationError.amountTooLarge("일일 최대 거래 금액을 초과했습니다")
        }
        
        // 의심스러운 패턴 감지 (같은 금액 연속 10회 이상)
        let recentSame = getRecentSameAmountTransactions(transaction.amount)
        guard recentSame.count < 10 else {
            throw ValidationError.suspiciousPattern("같은 금액의 거래가 너무 많습니다. 확인해주세요.")
        }
    }
}
```

### 🔄 데이터 정합성 유지
```swift
class DataIntegrityManager {
    // 카테고리별 월 합계 자동 계산 및 검증
    func recalculateMonthlyTotals(for month: Date) {
        let transactions = getMonthlyTransactions(for: month)
        let categoryTotals = Dictionary(grouping: transactions) { $0.category }
            .mapValues { $0.reduce(0) { $0 + $1.amount } }
        
        // 계산된 합계와 저장된 합계 비교
        validateMonthlySummary(categoryTotals, for: month)
    }
    
    // 예산 상태 일관성 검사
    func validateBudgetConsistency() {
        let categories = ExpenseCategory.allCases
        for category in categories {
            let spent = getCurrentMonthSpent(for: category)
            let budget = getBudget(for: category)
            let status = calculateBudgetStatus(spent: spent, budget: budget)
            
            // 상태 불일치 시 수정
            updateBudgetStatus(for: category, status: status)
        }
    }
}
```

## 📱 사용자 경험 최적화 규칙

### ⚡ 성능 최적화
```swift
struct PerformanceRules {
    // 메모리 관리
    static let maxTransactionsInMemory = 1000    // 최대 1000개 거래만 메모리에 유지
    static let dataRetentionPeriod = 5 * 365     // 5년간 데이터 보관
    
    // 캐시 정책
    static let statisticsCacheExpiry = 3600      // 통계 캐시 1시간
    static let categoryCacheExpiry = 86400       // 카테고리 캐시 24시간
    
    // 자동 정리
    func performMaintenanceIfNeeded() {
        let lastMaintenance = getLastMaintenanceDate()
        let daysSinceLastMaintenance = Calendar.current.dateComponents([.day], 
                                                                       from: lastMaintenance, 
                                                                       to: Date()).day ?? 0
        
        if daysSinceLastMaintenance >= 7 {  // 주 1회 정리
            cleanupOldData()
            recalculateStatistics()
            optimizeDatabase()
        }
    }
}
```

### 🎨 UI 최적화 규칙
```swift
struct UIOptimizationRules {
    // 카테고리 색상 매핑 (일관성 유지)
    static let categoryColors: [ExpenseCategory: Color] = [
        .food: .orange,          // 식비 - 주황색
        .transport: .blue,       // 교통비 - 파란색
        .shopping: .pink,        // 쇼핑 - 분홍색
        .cafe: .brown,          // 카페 - 갈색
        .culture: .purple,       // 문화 - 보라색
        .medical: .green,        // 의료 - 초록색
        .utility: .gray,         // 공과금 - 회색
        .education: .indigo,     // 교육 - 남색
        .other: .secondary       // 기타 - 보조색
    ]
    
    // 금액 표시 규칙
    static func formatAmountForDisplay(_ amount: Int, type: TransactionType) -> (text: String, color: Color) {
        let formattedAmount = AmountHandling.formatAmount(amount)
        let color: Color = type == .expense ? .red : .green
        return (formattedAmount, color)
    }
    
    // 터치 영역 최적화
    static let minimumTouchTarget: CGFloat = 44.0    // 최소 44pt 터치 영역
    static let preferredTouchTarget: CGFloat = 56.0  // 권장 56pt 터치 영역
}
```

## 🔔 알림 및 피드백 규칙

### 📢 예산 알림 정책
```swift
enum NotificationPolicy {
    case immediate      // 즉시 알림 (예산 초과)
    case daily         // 일일 요약 (선택적)
    case weekly        // 주간 요약 (선택적)
    case monthly       // 월간 분석 (기본)
    
    var defaultTime: DateComponents {
        switch self {
        case .daily:   return DateComponents(hour: 21, minute: 0)   // 오후 9시
        case .weekly:  return DateComponents(weekday: 1, hour: 20)  // 일요일 오후 8시  
        case .monthly: return DateComponents(day: 1, hour: 9)       // 매월 1일 오전 9시
        default:       return DateComponents()
        }
    }
}
```

### 🎯 성취 시스템
```swift
struct AchievementSystem {
    enum Achievement {
        case consistentRecording(days: Int)     // 연속 기록
        case budgetCompliance(months: Int)      // 예산 준수
        case savingGoal(amount: Int)           // 절약 목표 달성
        case categoryBalance                    // 균형잡힌 소비
    }
    
    // 마일스톤 체크
    func checkAchievements(for user: User) -> [Achievement] {
        var achievements: [Achievement] = []
        
        // 연속 기록 체크
        let consecutiveDays = getConsecutiveRecordingDays(for: user)
        if [7, 30, 100].contains(consecutiveDays) {
            achievements.append(.consistentRecording(days: consecutiveDays))
        }
        
        // 예산 준수 체크  
        let consecutiveMonths = getBudgetComplianceMonths(for: user)
        if [1, 3, 6].contains(consecutiveMonths) {
            achievements.append(.budgetCompliance(months: consecutiveMonths))
        }
        
        return achievements
    }
}
```

## 🚫 제외되는 기능들

### ❌ 스코프 외 기능
- **복잡한 투자 관리**: 주식, 펀드, 암호화폐 포트폴리오
- **은행 API 연동**: 계좌 잔액 조회, 자동 거래 내역 가져오기
- **신용카드 연동**: 카드사 API를 통한 자동 입력
- **다중 계좌 관리**: 여러 은행 계좌 통합 관리
- **세무 처리**: 종합소득세, 부가가치세 계산
- **가족 공유**: 여러 사용자 간 가계부 공유

### ⚠️ 피해야 할 복잡성
- 20개 이상의 세분화된 카테고리
- 복잡한 태그 시스템이나 다중 카테고리 분류
- 지나치게 상세한 메모나 첨부파일 기능
- 위치 기반 자동 분류 (개인정보 이슈)
- 실시간 환율 적용이나 해외 거래 관리

이러한 비즈니스 규칙들은 MoneyManager 앱이 사용자의 핵심 니즈인 "빠른 기록, 명확한 분류, 실용적인 예산 관리"에 집중할 수 있도록 가이드합니다.