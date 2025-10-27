# 🚀 Xcode 프로젝트 생성 가이드

> **추천 워크플로우**: AI 개발 시작 전에 먼저 Xcode 프로젝트를 생성하세요.
> 이렇게 하면 파일명 충돌 없이 정확한 프로젝트 구조로 개발할 수 있습니다.

## 📋 단계별 프로젝트 생성

### 1️⃣ Xcode 실행 및 프로젝트 생성
1. **Xcode 실행**
2. **"Create New Project"** 선택
3. **iOS 탭** → **"App"** 선택 → **Next** 클릭

### 2️⃣ 프로젝트 설정
- **Product Name**: `[원하는 앱 이름]` (예: OnePageTodo, MyFitnessApp)
- **Interface**: **SwiftUI** 선택
- **Language**: **Swift** 선택
- **Use Core Data**: **체크** ✅
- **Include Tests**: **체크** ✅ (권장)

### 3️⃣ 저장 위치 및 생성
1. **저장 위치** 선택
2. **"Create"** 클릭
3. **빌드 확인**: `⌘ + B` 눌러서 에러 없이 빌드되는지 확인

## ✅ 생성 완료 확인사항

### 필수 파일 확인
- [ ] `[앱이름].xcodeproj` 파일 존재
- [ ] `[앱이름]App.swift` 파일 생성됨
- [ ] `ContentView.swift` 파일 생성됨
- [ ] `[앱이름].xcdatamodeld` 파일 생성됨 (Core Data)
- [ ] `Assets.xcassets` 폴더 존재

### ✅ 생성 완료 후
프로젝트 생성이 완료되면 AI에게 **"프로젝트 생성 완료!"**라고 알려주세요.
AI가 생성된 프로젝트를 감지하고 정확한 이름으로 코드를 생성합니다.

### 빌드 테스트
```bash
# Xcode에서 다음 단축키로 빌드 테스트
⌘ + B  # 빌드
⌘ + R  # 실행
```

## ⚠️ 주의사항

### ❌ 하지 말아야 할 것
- AI가 직접 `.xcodeproj` 파일을 생성하거나 수정
- 텍스트 에디터로 프로젝트 파일 직접 편집
- 손상된 프로젝트 파일을 계속 사용
- 기존 폴더와 같은 이름으로 프로젝트 생성 (충돌 발생)

### ✅ 올바른 방법
- 항상 Xcode를 통한 정상적인 프로젝트 생성
- AI는 Swift 코드 파일만 생성/수정
- 프로젝트 설정 변경은 Xcode UI를 통해서만
- 파일명 충돌 사전 체크 및 해결

## 🔄 AI 개발 워크플로우

### ✅ 추천 순서 (문제 없는 방법)
1. **먼저 Xcode 프로젝트 생성** (위 가이드 참조)
2. **AI에게 "프로젝트 생성 완료!" 알림**
3. **AI가 자동으로 프로젝트 감지 후 정확한 이름으로 개발**

### 🔧 프로젝트를 나중에 생성하는 경우
AI가 먼저 코드를 생성했다면:
1. **임시 폴더**: AI_[앱이름] 형태로 파일들이 생성됨
2. **나중에 이동**: FILE_MIGRATION_RULES.md 참조하여 Xcode 프로젝트로 파일 이동
3. **안전 보장**: 백업 생성 후 이동하므로 파일 손실 없음

## 💡 간단한 팁

### 이름 충돌 방지
- **가장 쉬운 방법**: AI 개발 시작 전에 먼저 Xcode 프로젝트 생성
- **충돌 발생 시**: AI가 자동으로 `AI_[앱이름]` 폴더에 생성하므로 걱정 없음

## 🔧 문제 해결

### 프로젝트가 열리지 않는 경우
```
Error: "The project is damaged and cannot be opened"
```

**해결책**:
1. 현재 프로젝트 백업
2. Xcode에서 새 프로젝트 생성
3. 기존 Swift 파일들을 새 프로젝트로 복사
4. AI에게 누락된 코드 재생성 요청

### 빌드 에러가 발생하는 경우
1. **Clean Build Folder**: `⌘ + Shift + K`
2. **Derived Data 삭제**: Xcode → Preferences → Locations → Derived Data
3. **프로젝트 재빌드**: `⌘ + B`

## 📞 AI와의 협업 방법

### 프로젝트 생성 후 AI에게 알리는 방법
```
프로젝트 생성 완료!
```
이 한 마디면 충분합니다. AI가 자동으로 프로젝트를 감지하고 정확한 이름으로 개발을 시작합니다.

### ✅ AI가 하는 일
- 기존 프로젝트명 자동 감지
- 정확한 폴더 구조로 Swift 파일 생성
- Clean Architecture 기반 코드 구현
- 테스트 코드 작성

### ⚠️ 주의사항
- AI는 .xcodeproj 파일을 수정하지 않음 (안전성 보장)
- 프로젝트 설정은 사용자가 Xcode에서 직접 변경

---

## 📁 파일 이동이 필요한 경우

### 중간/나중에 프로젝트를 생성하는 상황
AI가 이미 Swift 파일들을 생성한 후에 Xcode 프로젝트를 만드는 경우:

1. **자동 감지**: AI가 기존 파일들을 자동으로 감지
2. **파일 이동**: FILE_MIGRATION_RULES.md에 따라 안전하게 이동
3. **백업 생성**: 기존 파일들을 자동으로 백업
4. **Xcode 추가**: 사용자가 Xcode에서 파일들을 프로젝트에 추가

### 파일 이동 과정
```
기존 상태:
📁 MyProject/
   📄 ContentView.swift
   📄 TodoViewModel.swift
   📄 TodoItem.swift

AI 작업 후:
📁 MyProject/
   📁 MyProject_Backup_20240120_153000/  ← 백업 폴더
   📁 MyProject.xcodeproj/                ← Xcode 프로젝트
   📁 MyProject/                          ← 새 소스 폴더
      📁 Presentation/Views/
         📄 ContentView.swift
      📁 Presentation/ViewModels/
         📄 TodoViewModel.swift
```

**프로젝트 생성이 완료되면 AI 개발을 시작할 수 있습니다! 🎉**