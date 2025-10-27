# 🚨 즉시 해결 가이드 - OnePageTodo 프로젝트

> **현재 상황**: `/Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo` 프로젝트의 Core Data 오류 해결

## 📊 현재 발생한 오류들

### 1️⃣ TodoViewModel을 찾을 수 없음
```
/Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo/OnePageTodo/ContentView.swift:6:41 
Cannot find type 'TodoViewModel' in scope
```

### 2️⃣ Item Entity를 찾을 수 없음
```
/Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo/OnePageTodo/Persistence.swift:18:27 
Cannot find 'Item' in scope
```

## 🎯 즉시 해결 방법

### Step 1: Xcode에서 파일 추가 확인
```bash
# Xcode를 열고 다음 파일들이 프로젝트에 추가되어 있는지 확인:
1. TodoViewModel.swift (Presentation/ViewModels/ 폴더 안)
2. TodoItem.swift (Domain/Entities/ 폴더 안)
3. 기타 Domain, Data 폴더의 모든 Swift 파일들
```

**해결 방법**:
1. Xcode에서 프로젝트 네비게이터 열기
2. 프로젝트 루트 우클릭 → "Add Files to 'OnePageTodo'"
3. 누락된 폴더들 (Presentation, Domain, Data) 선택
4. "Add" 클릭

### Step 2: Entity 이름 불일치 해결

**현재 상황 확인**:
```bash
# 터미널에서 실행하여 실제 Entity 이름 확인
find /Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo -name "*.xcdatamodeld"
```

**Persistence.swift 수정**:
현재 파일에서 `Item`을 `TodoEntity`로 변경:

```swift
// 기존 (오류 발생)
let newItem = Item(context: viewContext)

// 수정 후
let newTodo = TodoEntity(context: viewContext)
newTodo.id = UUID()
newTodo.title = "샘플 할 일"
newTodo.isCompleted = false
newTodo.createdAt = Date()
newTodo.updatedAt = Date()
```

### Step 3: 중복 파일 정리

**현재 중복 상황**:
```
📁 OnePageTodo/
   📄 Persistence.swift                           ← Xcode 기본 (문제 있음)
   📁 Data/CoreData/
      📄 PersistenceController.swift              ← AI 생성 (올바름)
```

**해결 방법**:
1. **기존 Persistence.swift 삭제** 또는 이름 변경
2. **AI 생성 PersistenceController.swift 사용**

```bash
# 터미널에서 실행
cd /Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo/OnePageTodo
mv Persistence.swift Persistence_Original.swift  # 백업용 이름 변경
```

### Step 4: Import 문제 해결

**ContentView.swift 확인**:
```swift
import SwiftUI
import CoreData

// TodoViewModel은 같은 타겟 내에 있으므로 별도 import 불필요
// 단, Xcode 프로젝트에 제대로 추가되어야 함
```

### Step 5: Clean Build 및 테스트

```bash
# Xcode에서 실행
1. ⌘ + Shift + K  (Clean Build Folder)
2. ⌘ + B          (Build)
3. 오류 확인 및 해결
```

## 🔧 자동 진단 스크립트

### 파일 존재 여부 확인
```bash
#!/bin/bash
echo "=== OnePageTodo 프로젝트 진단 ==="
PROJECT_PATH="/Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo"

echo "1. Swift 파일 목록:"
find "$PROJECT_PATH" -name "*.swift" | sort

echo -e "\n2. Entity 참조 확인:"
echo "Item 참조:"
grep -r "Item(" "$PROJECT_PATH" --include="*.swift" 2>/dev/null || echo "없음"

echo "TodoEntity 참조:"
grep -r "TodoEntity(" "$PROJECT_PATH" --include="*.swift" 2>/dev/null || echo "없음"

echo -e "\n3. Core Data 모델 파일:"
find "$PROJECT_PATH" -name "*.xcdatamodeld"

echo -e "\n4. Persistence 관련 파일:"
find "$PROJECT_PATH" -name "*ersistence*" -type f
```

### Entity 이름 일치화 스크립트
```bash
#!/bin/bash
echo "=== Entity 이름 통일 작업 ==="
PROJECT_PATH="/Users/yeogangsu/Desktop/PRD/3_test/1/OnePageTodo"

# Item을 TodoEntity로 일괄 변경 (백업 생성 후)
echo "백업 생성 중..."
cp -r "$PROJECT_PATH" "${PROJECT_PATH}_backup_$(date +%Y%m%d_%H%M%S)"

echo "Entity 이름 통일 중..."
# Persistence.swift에서 Item 참조를 TodoEntity로 변경
sed -i '' 's/= Item(/= TodoEntity(/g' "$PROJECT_PATH/OnePageTodo/Persistence.swift" 2>/dev/null

echo "완료! 이제 Xcode에서 빌드 테스트하세요."
```

## ⚡ 빠른 해결 순서 (5분 해결)

1. **Xcode 열기**: OnePageTodo.xcodeproj
2. **파일 추가**: Presentation, Domain, Data 폴더 추가
3. **Persistence.swift 수정**: `Item` → `TodoEntity`
4. **Clean Build**: ⌘ + Shift + K
5. **빌드 테스트**: ⌘ + B

## 🆘 여전히 문제가 있다면

### Core Data Model 직접 확인
1. Xcode에서 `OnePageTodo.xcdatamodeld` 파일 열기
2. Entity 이름이 정확히 무엇인지 확인
3. 모든 Swift 파일에서 동일한 이름 사용

### 프로젝트 재생성 고려
심각한 문제가 계속되면:
1. 현재 Swift 파일들 백업
2. 새로운 Xcode 프로젝트 생성
3. Swift 파일들을 새 프로젝트로 복사
4. Entity 이름 일치시키기

---

**이 가이드로 5분 내에 문제를 해결할 수 있습니다! 🎉**