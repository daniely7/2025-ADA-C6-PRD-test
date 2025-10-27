# 📁 파일 이동 규칙 (FILE_MIGRATION_RULES.md)

> **목적**: 프로젝트 생성이 중간/나중에 이루어질 때 기존 AI 생성 파일들을 Xcode 프로젝트로 안전하게 이동하는 규칙

## 🎯 사용 시나리오

### 1️⃣ 일반적인 상황
- AI가 먼저 Xcode 프로젝트 생성을 시도했지만 실패
- 사용자가 나중에 직접 Xcode 프로젝트를 생성
- 기존에 생성된 Swift 파일들을 새 프로젝트로 이동 필요

### 2️⃣ 중간 생성 상황
- AI가 Swift 파일들을 이미 생성한 상태
- 개발 중간에 Xcode 프로젝트가 필요해짐
- 작업 중인 파일들을 손실 없이 이동 필요

## 📋 파일 이동 체크리스트

### Step 1: 현재 상황 파악
- [ ] **기존 파일 확인**: AI가 생성한 Swift 파일들 목록 작성
- [ ] **Xcode 프로젝트 확인**: 사용자가 생성한 프로젝트 경로 확인  
- [ ] **대소문자 매칭 검증**: {{APP_NAME}} vs 실제 프로젝트명 케이싱 확인
- [ ] **충돌 파일 체크**: 같은 이름의 파일이 있는지 확인

### Step 2: 백업 생성
```bash
# 기존 AI 생성 파일들 백업
cp -r {{APP_NAME}} {{APP_NAME}}_Backup_$(date +%Y%m%d_%H%M%S)
```

### Step 3: 파일 매핑 및 이동
- [ ] **Domain 파일들**: `Domain/` → `{{APP_NAME}}/Domain/`
- [ ] **Presentation 파일들**: `Presentation/` → `{{APP_NAME}}/Presentation/`
- [ ] **Data 파일들**: `Data/` → `{{APP_NAME}}/Data/`
- [ ] **기타 Swift 파일들**: 루트의 `.swift` 파일들을 적절한 위치로

## 🗂️ 자동 파일 매핑 규칙

### Swift 파일 매핑
```
기존 위치 → 새 위치 (Xcode 프로젝트 내)

# 앱 엔트리
{{APP_NAME}}App.swift → {{APP_NAME}}/{{APP_NAME}}App.swift

# UI 레이어
ContentView.swift → {{APP_NAME}}/Presentation/Views/ContentView.swift
*RowView.swift → {{APP_NAME}}/Presentation/Views/
*ViewModel.swift → {{APP_NAME}}/Presentation/ViewModels/

# 도메인 레이어
*Entity.swift → {{APP_NAME}}/Domain/Entities/
*Service.swift → {{APP_NAME}}/Domain/UseCases/
*Repository.swift → {{APP_NAME}}/Domain/RepositoryInterfaces/

# 데이터 레이어
CoreData*.swift → {{APP_NAME}}/Data/Repositories/
PersistenceController.swift → {{APP_NAME}}/Data/CoreData/
*.xcdatamodeld → {{APP_NAME}}/Data/CoreData/
*MO.swift → {{APP_NAME}}/Data/CoreData/
```

### 리소스 파일 매핑
```
Assets.xcassets → {{APP_NAME}}/Assets.xcassets (병합)
Info.plist → {{APP_NAME}}/Info.plist (설정 병합)
```

## 🛠️ 이동 작업 수행 절차

### 1단계: 자동 감지 및 분류
```bash
# AI가 수행할 작업
echo "기존 파일들을 분석합니다..."

# 대소문자 불일치 감지 로직
echo "프로젝트명 케이싱 확인 중..."
XCODE_PROJECT=$(find . -name "*.xcodeproj" | head -1)
PROJECT_NAME=$(basename "$XCODE_PROJECT" .xcodeproj)
echo "실제 Xcode 프로젝트명: $PROJECT_NAME"

# {{APP_NAME}} 변수와 실제 프로젝트명 비교
if [ "{{APP_NAME}}" != "$PROJECT_NAME" ]; then
    echo "⚠️  케이싱 불일치 감지:"
    echo "템플릿: {{APP_NAME}}"
    echo "실제 프로젝트: $PROJECT_NAME"
    echo "모든 파일 경로를 실제 프로젝트명으로 조정합니다."
fi

# Swift 파일 목록 작성 (케이스 인센시티브)
find . -name "*.swift" -not -path "./*.xcodeproj/*"

# 폴더 구조 확인 (대소문자 구분)
ls -la | grep "^d"
```

### 2단계: 폴더 구조 생성 (Xcode 프로젝트 내)
```bash
# 실제 프로젝트명 사용하여 폴더 생성 (케이싱 정확)
ACTUAL_PROJECT_NAME="$PROJECT_NAME"

# 필요한 폴더 생성 (실제 케이싱 적용)
mkdir -p "$ACTUAL_PROJECT_NAME/Presentation/Views"
mkdir -p "$ACTUAL_PROJECT_NAME/Presentation/ViewModels"
mkdir -p "$ACTUAL_PROJECT_NAME/Domain/Entities"
mkdir -p "$ACTUAL_PROJECT_NAME/Domain/UseCases"
mkdir -p "$ACTUAL_PROJECT_NAME/Domain/RepositoryInterfaces"
mkdir -p "$ACTUAL_PROJECT_NAME/Data/Repositories"
mkdir -p "$ACTUAL_PROJECT_NAME/Data/CoreData"

echo "✅ 폴더 구조 생성 완료 (프로젝트명: $ACTUAL_PROJECT_NAME)"
```

### 3단계: 파일 이동 실행
```bash
# 실제 프로젝트명을 사용한 파일 이동 (케이싱 정확)
echo "파일 이동 시작 (타겟: $ACTUAL_PROJECT_NAME)"

# 스마트 매칭 로직: 대소문자 무관하게 폴더 찾기
find_project_folder() {
    # 현재 디렉토리에서 케이스 인센시티브하게 프로젝트 폴더 찾기
    find . -maxdepth 1 -type d -iname "*$(echo $ACTUAL_PROJECT_NAME | tr '[:upper:]' '[:lower:]')*" | head -1
}

TARGET_FOLDER=$(find_project_folder)
if [ -z "$TARGET_FOLDER" ]; then
    echo "❌ 타겟 폴더를 찾을 수 없습니다: $ACTUAL_PROJECT_NAME"
    exit 1
fi

echo "🎯 타겟 폴더 확인: $TARGET_FOLDER"

# 매핑 규칙에 따라 파일 이동 (실제 프로젝트명 사용)
if [ -f "ContentView.swift" ]; then
    mv ContentView.swift "$ACTUAL_PROJECT_NAME/Presentation/Views/"
    echo "✅ ContentView.swift 이동 완료"
fi

# ViewModel 파일들 이동
if ls *ViewModel.swift 1> /dev/null 2>&1; then
    mv *ViewModel.swift "$ACTUAL_PROJECT_NAME/Presentation/ViewModels/"
    echo "✅ ViewModel 파일들 이동 완료"
fi

# 기타 패턴 매칭 파일들 이동 (실제 케이싱 적용)
# ...추가 매핑 규칙들...
```

### 4단계: Xcode 프로젝트에 파일 추가
**⚠️ 중요**: AI가 직접 .xcodeproj 파일을 수정하지 않음!

사용자에게 안내:
```
"파일 이동이 완료되었습니다. 
이제 Xcode에서 다음 작업을 수행해주세요:

1. Xcode에서 프로젝트 열기
2. 프로젝트 네비게이터에서 우클릭 → "Add Files to '{{APP_NAME}}'"
3. 이동된 폴더들 선택 (Presentation, Domain, Data)
4. "Add" 클릭하여 프로젝트에 추가
5. 빌드 테스트: ⌘ + B"
```

## 🔧 대소문자 불일치 해결 전략

### 케이싱 불일치 시나리오
```
일반적인 문제:
AI 템플릿: {{APP_NAME}} = "OnePageTodo" (PascalCase)
실제 프로젝트: "onepagetodo" (lowercase)

결과: 폴더/파일 경로 불일치로 인한 이동 실패
```

### 자동 케이싱 보정 로직
```bash
# 1. 실제 프로젝트명 감지
XCODE_PROJECT=$(find . -name "*.xcodeproj" | head -1)
ACTUAL_NAME=$(basename "$XCODE_PROJECT" .xcodeproj)

# 2. 케이스 인센시티브 매칭
normalize_case() {
    echo "$1" | tr '[:upper:]' '[:lower:]'
}

# 3. 스마트 폴더 찾기
find_matching_folder() {
    local target_name=$(normalize_case "$1")
    find . -maxdepth 1 -type d | while read dir; do
        dir_name=$(basename "$dir")
        if [ "$(normalize_case "$dir_name")" = "$target_name" ]; then
            echo "$dir"
            return
        fi
    done
}

# 4. 경로 자동 보정
CORRECT_PATH=$(find_matching_folder "$ACTUAL_NAME")
echo "✅ 정확한 경로: $CORRECT_PATH"
```

### 케이싱 충돌 해결 우선순위
1. **Xcode 프로젝트명 우선**: .xcodeproj 파일명을 기준으로 함
2. **기존 폴더 보존**: 이미 존재하는 폴더 구조 유지
3. **자동 매핑**: 케이스 인센시티브 매칭으로 자동 해결
4. **사용자 확인**: 애매한 경우만 사용자에게 문의

## ⚠️ 충돌 해결 규칙

### 같은 이름 파일이 있는 경우
1. **기존 파일 보존**: `ContentView.swift` → `ContentView_Original.swift`
2. **AI 파일 적용**: AI가 생성한 파일을 메인으로 사용
3. **사용자 확인**: "기존 ContentView.swift와 AI 생성 파일 중 어느 것을 사용하시겠습니까?"

### 폴더 구조 충돌
1. **병합 우선**: 기존 폴더와 AI 폴더 내용 병합
2. **중복 파일**: 위의 파일 충돌 규칙 적용
3. **사용자 선택**: 충돌이 많은 경우 사용자에게 선택권 제공

### 🚨 Core Data 관련 특별 충돌 해결

#### Persistence 파일 중복 문제
```
일반적 충돌:
📁 {{APP_NAME}}/
   📄 Persistence.swift                    ← Xcode 기본 생성
   📁 Data/CoreData/
      📄 PersistenceController.swift       ← AI 생성
```

**해결 규칙**:
1. **Xcode 기본 파일 확인**: `Persistence.swift`에서 사용하는 Entity 이름 확인
2. **Entity 불일치 감지**: `Item` vs `TodoEntity` 등 불일치 확인
3. **통합 결정**: 하나의 파일로 통합 (AI 생성 버전 우선)
4. **Entity 이름 통일**: 모든 파일에서 동일한 Entity 이름 사용

#### Core Data Model (.xcdatamodeld) 충돌
```
문제 시나리오:
- Xcode가 생성한 모델: "Item" Entity
- AI가 참조하는 모델: "TodoEntity" Entity
```

**해결 순서**:
1. **.xcdatamodeld 파일 확인**: 실제 Entity 이름 파악
2. **코드 일치화**: 모든 Swift 파일을 실제 Entity 이름에 맞춤
3. **Import 문제 해결**: 누락된 import 구문 추가
4. **참조 업데이트**: 모든 Entity 참조를 일관되게 수정

## 📊 이동 작업 로깅

### 작업 전 상태 기록
```json
{
  "timestamp": "2024-01-20T10:30:00Z",
  "action": "file_migration_start",
  "source_files": ["ContentView.swift", "TodoViewModel.swift", ...],
  "target_project": "{{APP_NAME}}.xcodeproj",
  "backup_created": "{{APP_NAME}}_Backup_20240120_103000"
}
```

### 작업 후 상태 기록
```json
{
  "timestamp": "2024-01-20T10:35:00Z",
  "action": "file_migration_complete",
  "moved_files": {
    "ContentView.swift": "{{APP_NAME}}/Presentation/Views/ContentView.swift",
    "TodoViewModel.swift": "{{APP_NAME}}/Presentation/ViewModels/TodoViewModel.swift"
  },
  "conflicts_resolved": ["ContentView.swift renamed to ContentView_Original.swift"],
  "status": "success"
}
```

## 🎯 성공 기준

### 이동 작업 성공 조건
- [ ] 모든 Swift 파일이 적절한 위치로 이동됨
- [ ] 파일 내용 손실 없음
- [ ] Xcode 프로젝트에서 빌드 성공
- [ ] 기존 기능 정상 작동
- [ ] 백업 파일 생성 완료

### 실패 시 롤백 절차
1. **백업에서 복원**: `cp -r {{APP_NAME}}_Backup_* {{APP_NAME}}`
2. **Xcode 프로젝트 리셋**: 사용자가 새로 생성
3. **재시도**: 충돌 원인 해결 후 다시 시도

---

**이 규칙을 통해 언제든지 안전하게 파일을 이동할 수 있습니다! 🚀**