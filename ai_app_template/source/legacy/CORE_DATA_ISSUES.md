# 🔧 Core Data 문제 해결 가이드 (CORE_DATA_ISSUES.md)

> **목적**: Core Data 관련 일반적인 문제들과 해결책을 체계적으로 정리

## 🚨 가장 흔한 Core Data 문제들

### 1️⃣ Entity 이름 불일치 문제
**증상**: `Cannot find 'Item' in scope` 또는 `Cannot find type 'TodoEntity' in scope`

**원인**:
- Xcode가 기본 생성: `Item` Entity
- AI가 참조: `TodoEntity` Entity  
- 두 이름이 섞여서 사용됨

**해결책**:
```swift
// 문제가 되는 코드
let newItem = Item(context: viewContext)  // ❌ Entity 이름 불일치

// 올바른 코드
let newTodo = TodoEntity(context: viewContext)  // ✅ 실제 Entity 이름 사용
```

### 2️⃣ ViewModel Import 누락 문제
**증상**: `Cannot find type 'TodoViewModel' in scope`

**원인**:
- 파일 이동 시 import 경로가 깨짐
- 같은 모듈 내 파일이지만 폴더 구조 변경으로 참조 불가

**해결책**:
```swift
// ContentView.swift에서
import SwiftUI
import CoreData
// TodoViewModel은 같은 모듈 내에 있으므로 별도 import 불필요
// 단, 파일이 제대로 Xcode 프로젝트에 추가되어야 함
```

### 3️⃣ Persistence 파일 중복 문제
**증상**: 두 개의 Persistence 관련 파일이 다른 Entity를 참조

**원인**:
```
📁 OnePageTodo/
   📄 Persistence.swift              ← Xcode 생성 (Item 사용)
   📁 Data/CoreData/
      📄 PersistenceController.swift ← AI 생성 (TodoEntity 사용)
```

**해결책**: 하나로 통합하고 Entity 이름 일치시키기

## 📋 Core Data 검증 체크리스트

### 프로젝트 설정 확인
- [ ] **.xcdatamodeld 파일 존재**: Core Data 모델 파일이 있는가?
- [ ] **Entity 이름 확인**: 실제 모델에서 정의된 Entity 이름은?
- [ ] **코드 일치성**: 모든 Swift 파일이 같은 Entity 이름 사용?
- [ ] **Import 구문**: 필요한 import CoreData가 모든 파일에 있는가?

### 파일 구조 검증
- [ ] **Persistence 파일 단일화**: Persistence 관련 파일이 하나만 존재?
- [ ] **Entity 클래스 생성**: Core Data가 자동 생성한 Entity 클래스 확인
- [ ] **ViewModel 연결**: ViewModel이 Xcode 프로젝트에 제대로 추가됨?

### 빌드 전 최종 확인
- [ ] **Clean Build**: `⌘ + Shift + K`로 빌드 폴더 정리
- [ ] **Entity 일관성**: 모든 파일에서 동일한 Entity 이름 사용
- [ ] **Import 완성도**: 누락된 import 문 없음

## 🛠️ 문제별 자동 해결 스크립트

### Entity 이름 불일치 자동 감지
```bash
# 프로젝트 내 Entity 참조 불일치 찾기
echo "=== Entity 참조 불일치 검사 ==="
echo "Item 참조 파일들:"
grep -r "Item(" . --include="*.swift" | grep -v "TodoItem"

echo "TodoEntity 참조 파일들:"  
grep -r "TodoEntity(" . --include="*.swift"

echo "=== .xcdatamodeld 파일의 실제 Entity 확인 필요 ==="
find . -name "*.xcdatamodeld"
```

### 중복 Persistence 파일 감지
```bash
# Persistence 관련 파일 찾기
echo "=== Persistence 관련 파일들 ==="
find . -name "*ersistence*" -type f
echo ""
echo "=== Core Data 관련 import 파일들 ==="
grep -l "import CoreData" . --include="*.swift" 2>/dev/null
```

## 🎯 표준 Core Data 패턴

### 올바른 Entity 네이밍 컨벤션
```
❌ 피해야 할 이름:
- Item (너무 일반적)
- Entity (예약어와 혼동)
- Data (모호함)

✅ 권장하는 이름:
- TodoEntity (명확하고 구체적)
- TodoItem (Domain model과 구분)
- TaskEntity (역할이 명확)
```

### 표준 Persistence 구조
```swift
// PersistenceController.swift (권장 파일명)
import CoreData

struct PersistenceController {
    static let shared = PersistenceController()
    
    static var preview: PersistenceController = {
        let result = PersistenceController(inMemory: true)
        let viewContext = result.container.viewContext
        
        // 미리보기 데이터 생성
        let sampleTodo = TodoEntity(context: viewContext)  // ← 실제 Entity 이름 사용
        sampleTodo.id = UUID()
        sampleTodo.title = "샘플 할 일"
        sampleTodo.isCompleted = false
        sampleTodo.createdAt = Date()
        
        return result
    }()
    
    let container: NSPersistentContainer
    
    init(inMemory: Bool = false) {
        container = NSPersistentContainer(name: "DataModel")  // ← .xcdatamodeld 이름과 일치
        // ... 나머지 설정
    }
}
```

## 🔄 문제 해결 워크플로우

### 단계 1: 문제 진단
1. 빌드 에러 메시지 정확히 파악
2. 관련 파일들의 Entity 이름 확인
3. .xcdatamodeld에서 실제 Entity 이름 확인

### 단계 2: 통합 작업
1. **중복 파일 제거**: Persistence 관련 파일 하나로 통합
2. **Entity 이름 통일**: 모든 파일에서 동일한 이름 사용
3. **Import 정리**: 누락된 import 구문 추가

### 단계 3: 검증
1. **Clean Build**: `⌘ + Shift + K`
2. **빌드 테스트**: `⌘ + B`
3. **실행 테스트**: `⌘ + R`

### 단계 4: 문제 지속 시
1. **Derived Data 삭제**: Xcode → Preferences → Locations
2. **프로젝트 재시작**: Xcode 완전 종료 후 재시작
3. **Entity 클래스 재생성**: Core Data Inspector에서 Codegen 확인

## ⚠️ 예방 수칙

### 프로젝트 생성 시
1. **Entity 이름 미리 결정**: 프로젝트 시작 전에 Entity 이름 확정
2. **일관된 네이밍**: 모든 템플릿에서 동일한 Entity 이름 사용
3. **검증 단계 포함**: 파일 이동 후 반드시 빌드 테스트

### 템플릿 사용 시
1. **{{CORE_DATA_ENTITY}} 변수 사용**: 하드코딩된 Entity 이름 피하기
2. **자동 검증 스크립트**: 파일 이동 후 자동으로 일관성 검사
3. **백업 필수**: 모든 변경 전 백업 생성

---

**이 가이드로 Core Data 문제를 체계적으로 해결할 수 있습니다! 🎉**