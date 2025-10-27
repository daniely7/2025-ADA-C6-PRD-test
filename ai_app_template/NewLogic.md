# NewLogic.md — AI App Template 전체 시스템 로직 분석

> **목적**: ai_app_template의 전체 실행 로직과 각 단계별 MD 파일 역할, 룰 확인 과정을 상세 분석

---

## 🎯 시스템 개요

**ai_app_template**은 iOS 앱을 완전 자동으로 개발하는 **3단계 시스템**입니다:

```
사용자 입력: "ai_app_template을 기반으로 앱을 개발해줘!"
↓
1단계: PREFLIGHT (환경 검증) → A/B/C 모드 선택
2단계: APP_DISCOVERY (요구사항 수집) → 8단계 맞춤형 분석  
3단계: CLAUDE_TEMPLATE (실제 개발) → SwiftUI + Core Data 앱 완성
```

### 핵심 특징
- **완전 자동화**: 사용자는 질문 답변만, AI가 모든 코드 생성
- **맞춤형 개발**: 사용자별 요구사항 반영한 커스텀 앱
- **전문적 구조**: Clean Architecture + MVVM + 테스트 자동 생성

---

## 📂 파일 구조와 역할

### 🚀 메인 실행 파일들 (루트)
```
ai_app_template/
├── 01_PREFLIGHT.md        # 환경 검증 (A/B/C 모드)
├── 02_APP_DISCOVERY.md    # 8단계 맞춤형 요구사항 수집
├── 03_CLAUDE_TEMPLATE.md  # 실제 iOS 앱 개발 실행
├── README.md              # 사용자 가이드
└── source/                # AI가 참조하는 모든 리소스
```

### 📚 source/ 디렉토리 구조
```
source/
├── core/                  # AI 의사결정 시스템
│   ├── DECISION_MATRIX.md       # 의사결정 기준과 우선순위
│   ├── ADAPTATION_RULES.md      # 상황별 적응 규칙 (IF-THEN)
│   ├── QUALITY_GATES.md         # 품질 기준 (성능/안정성/접근성)
│   ├── VALIDATION_CHECKLIST.md  # 검증 시나리오
│   └── LEARNING_SIGNALS.md      # 성공/실패 지표
├── examples/              # 완성된 예시 앱들
│   ├── todo_app/         # 할일 관리 앱
│   ├── fitness_app/      # 운동 기록 앱
│   └── finance_app/      # 가계부 앱
├── archive/               # 기존 시스템 아카이브
├── legacy/                # 사용하지 않는 파일들
└── templates/             # 기타 템플릿들
```

---

## 🔄 전체 시나리오별 MD 파일 매핑

### **시나리오 A: 완전 커스텀 앱 개발** 🎨

#### **1단계: 환경 검증**
```
사용자: "ai_app_template을 기반으로 앱을 개발해줘!"
↓
AI 읽는 파일: 01_PREFLIGHT.md
↓
AI 행동: A/B/C 모드 선택지 제시
- A: 완전 개발 환경 ✅ (Xcode 있음, 2-3시간 투입 가능)
- B: 빠른 테스트 환경 🧪 (30분-1시간만 가능)
- C: 정보 확인만 ❓ (정보만 확인하고 싶음)
↓
사용자: "A 선택"
↓
AI: 개발 방식 선택 제시
1. 기존 예시 템플릿 활용 (빠름)
2. 맞춤형 새 앱 개발 (완전 커스텀) ← 선택
3. 학습/체험 모드 (간단)
```

#### **2단계: 맞춤형 요구사항 수집**
```
사용자: "2번 맞춤형 새 앱 개발"
↓
AI 읽는 파일: 02_APP_DISCOVERY.md
↓
AI 행동: 8단계 인터랙티브 질문 시작
Step 1: "어떤 앱 카테고리? 1.생산성 2.피트니스 3.금융..."
Step 2: "생산성 앱 중에서? 1.할일관리 2.메모 3.시간관리..."
Step 3: "주요 사용자는? 연령대/라이프스타일/기술수준"
Step 4: "핵심 가치 3가지? 속도/분석/사용자경험/기능깊이..."
Step 5: "특별 요구사항? 음성입력/다크모드/위젯지원..."
↓
수집 완료: 모든 정보 → 맞춤형 개발 시작
```

#### **3단계: 실제 개발 실행** 
```
AI 읽는 파일: 03_CLAUDE_TEMPLATE.md
↓
AI 행동: "Xcode 프로젝트 먼저 생성하세요" 안내
↓
사용자: "프로젝트 생성 완료!"
↓
AI: 부트 프로토콜 시작 → 11단계 자동 읽기 순서 실행
```

#### **AI 부트 프로토콜 (11단계 자동 읽기)**
```
1. 03_CLAUDE_TEMPLATE.md 전부 읽기 (현재 문서)
2. USER_CONTEXT.md 읽기 → 사용자 맥락과 의도 파악
3. BUSINESS_RULES.md 읽기 → 비즈니스 로직 커스터마이징
4. DECISION_MATRIX.md 읽기 → 의사결정 기준과 우선순위 확정
5. ADAPTATION_RULES.md 읽기 → 상황별 적응 규칙 파악
6. features/*.md 번호순 읽기 → 요구사항과 DoD 수집
7. Tasks.md 읽기 → 크리티컬 패스 실행 계획 확정
8. Test.md 읽기 → 자동/수동 테스트, A11y ID, DoD 고정
9. QUALITY_GATES.md 읽기 → 품질 기준 확정
10. VALIDATION_CHECKLIST.md 읽기 → 검증 시나리오 준비
11. 부트 프로토콜 즉시 수행 → 실제 코드 생성 시작
```

### **시나리오 B: 기존 예시 활용** ⚡

#### **빠른 개발 경로**
```
사용자: "ai_app_template을 기반으로 앱을 개발해줘!"
↓
AI: 01_PREFLIGHT.md 읽음 → A 모드 선택
↓
사용자: "A 선택 → 1번 기존 예시 템플릿 활용"
↓
AI: source/examples/ 목록 제시
- 📋 todo_app (할일 관리)
- 💪 fitness_app (운동 기록)  
- 💰 finance_app (가계부)
↓
사용자: "todo_app 선택"
↓
AI: 03_CLAUDE_TEMPLATE.md → 직접 개발 시작
  - todo_app/USER_CONTEXT.md → 기존 맥락 활용
  - todo_app/features/*.md → 검증된 요구사항
  - core/ 파일들로 품질 보장
```

### **시나리오 C: 빠른 테스트** 🧪

#### **학습/체험 모드**
```
사용자: "ai_app_template을 기반으로 앱을 개발해줘!"
↓
AI: 01_PREFLIGHT.md 읽음 → B 모드 선택
↓
사용자: "B 선택 (빠른 테스트 환경)"
↓
AI 행동:
- source/examples/ 앱 목록 제공
- 코드 구조 설명
- 시스템 이해 중심 가이드
- 실제 코드 생성 없이 학습 위주
```

---

## 🎯 커스텀 앱 Task List 생성 로직 (5단계)

### **1단계: 기본 템플릿 로드**
```
사용자 선택: "생산성 앱 → 할일관리"
↓
AI: source/examples/todo_app/Tasks.md 읽음
↓
기본 7단계 구조 확보:
0. 프로젝트 설정
1. 데이터 모델링 (Domain + CoreData)
2. 저장소 + 유스케이스
3. ViewModel
4. UI (SwiftUI)
5. 접합/정리
6. 테스트/품질
7. 산출/리포트
```

### **2단계: 사용자 맞춤 요구사항 반영**
```
02_APP_DISCOVERY.md에서 수집된 정보:
- 앱 종류: 운동 기록 앱
- 사용자: 30대 직장인
- 핵심 가치: 빠른 입력 중요
- 특별 요구사항: 사진 첨부, 음성 입력, 다크모드

AI 판단:
- 기본: todo_app/Tasks.md 구조 활용
- 추가: 사진 처리 기능 (Step 4에 ImagePicker 추가)
- 추가: 음성 입력 기능 (Speech Framework)
- 조정: 빠른 입력 우선 (UI 단순화)
- 추가: 다크모드 지원
```

### **3단계: features/*.md 분석하여 세부 태스크 생성**
```
AI가 읽는 파일들:
- features/F01_AddTodo.md → 운동 기록 추가 기능으로 변환
- features/F02_ToggleComplete.md → 운동 완료 토글
- features/F03_DeleteTodo.md → 운동 기록 삭제
- features/F04_EditTodo.md → 운동 기록 편집
- features/F05_Persistence.md → Core Data 영속성
- features/F06_Stats.md → 운동 통계

변환 결과:
F01 "할일 추가" → "운동 기록 추가: 운동명, 세트수, 무게, 시간, 사진"
- [ ] Domain/Entities/WorkoutRecord.swift 생성
- [ ] exerciseName(String), sets(Int), weight(Double), duration(TimeInterval), photo(Data?) 필드
- [ ] Core Data 모델에 동일 스키마 추가
```

### **4단계: DECISION_MATRIX.md로 우선순위 조정**
```
AI가 읽는 파일: source/core/DECISION_MATRIX.md

사용자 가중치 확인:
- 사용자 경험: 40% (높음) - 빠른 입력 중요
- 성능: 35% (높음) - 운동 중 빠른 기록
- 확장성: 5% (낮음) - 당장 필요 없음

Task 순서 조정:
- UI 구현을 앞당김 (사용자 경험 우선)
- 빠른 입력 UX에 집중
- 복잡한 확장 기능은 후순위로
```

### **5단계: ADAPTATION_RULES.md로 특별 요구사항 추가**
```
AI가 읽는 파일: source/core/ADAPTATION_RULES.md

IF-THEN 규칙 적용:
"IF 사진 첨부 요구 THEN ImagePicker + Core Data Binary 추가"
"IF 음성 입력 요구 THEN Speech Framework + 권한 설정 추가"
"IF 다크모드 요구 THEN UI 테마 시스템 추가"

추가 Task 생성:
- [ ] 4-1. PhotoPicker UI 구현
- [ ] 1-1. Core Data에 imageData(Binary) 필드 추가
- [ ] 2-1. 이미지 저장/로드 로직 구현
- [ ] 0-1. Speech Framework 권한 설정
- [ ] 3-1. SpeechRecognizer ViewModel 통합
- [ ] 4-2. 다크모드 테마 시스템
```

### **최종 생성된 커스텀 Task List 예시:**
```markdown
## 0. 프로젝트 설정
- [기본] Xcode 프로젝트, 폴더 구조
- [추가] Speech Framework 권한 설정
- [추가] Camera/PhotoLibrary 권한 설정

## 1. 데이터 모델링  
- [기본] WorkoutRecord 엔티티 (TodoItem에서 변환)
- [추가] 사진 데이터 필드 추가
- [추가] 음성 입력 메타데이터 필드

## 2. 저장소 + 유스케이스
- [기본] CRUD 로직
- [추가] 이미지 저장/로드 서비스
- [추가] 음성→텍스트 변환 서비스

## 3. ViewModel
- [조정] 빠른 입력에 최적화된 상태 관리
- [추가] ImagePicker 통합
- [추가] SpeechRecognizer 통합

## 4. UI
- [기본] 입력창, 리스트 (운동 기록용으로 조정)
- [추가] 사진 첨부 버튼
- [추가] 음성 입력 버튼  
- [추가] 다크모드 테마 시스템
- [우선순위 상승] 빠른 입력 UX (사용자 가중치 반영)

## 5. 접합/정리
- [기본] 의존성 방향 점검
- [추가] 권한 처리 검증

## 6. 테스트/품질
- [기본] Unit/UI 테스트
- [추가] 이미지 처리 테스트
- [추가] 음성 인식 테스트

## 7. 산출/리포트
- [기본] 결과 요약
- [추가] 커스텀 기능 동작 확인
```

---

## 📚 파일별 역할과 읽는 타이밍 상세 분석

### **🚀 메인 실행 파일들**

#### **01_PREFLIGHT.md** - 환경 검증
- **언제 읽나**: 사용자가 "ai_app_template을 기반으로 앱을 개발해줘!" 입력 시 제일 먼저
- **목적**: 성공적인 개발을 위한 환경 사전 검증
- **AI 행동**: A/B/C 모드 선택지 제시 → 개발 방식 결정

#### **02_APP_DISCOVERY.md** - 8단계 요구사항 수집
- **언제 읽나**: A 모드에서 "맞춤형 새 앱 개발" 선택 시
- **목적**: 모든 앱 개발 요청에 대해 체계적으로 요구사항 파악
- **AI 행동**: 8단계 인터랙티브 질문으로 완전한 맞춤 정보 수집

#### **03_CLAUDE_TEMPLATE.md** - 실제 개발 실행
- **언제 읽나**: 요구사항 수집 완료 후 또는 예시 템플릿 선택 시
- **목적**: 수집된 정보를 바탕으로 완전한 iOS 앱 개발
- **AI 행동**: Xcode 프로젝트 생성 안내 → 부트 프로토콜 시작

### **📚 source/examples/ - 예시 앱별 문서들**

#### **USER_CONTEXT.md** - 사용자 맥락 파악
- **언제 읽나**: 개발 시작 시 두 번째로 읽음 (부트 프로토콜 2단계)
- **목적**: 대상 사용자, 핵심 가치, 특별 요구사항 파악
- **예시**: "바쁜 직장인, 하루 10-20개 할일, 빠른 입력이 최우선"
- **AI 활용**: UI 설계와 기능 우선순위 결정에 반영

#### **BUSINESS_RULES.md** - 비즈니스 로직 커스터마이징
- **언제 읽나**: 사용자별 맞춤 기능 구현 전 (부트 프로토콜 3단계)
- **목적**: 분류 체계, 시간 관리, 알림 정책 등 비즈니스 규칙 정의
- **예시**: "프로젝트별 분류 필수, 매주 월요일 반복 할일 자동 생성"
- **AI 활용**: 카테고리 기능과 반복 알림 로직 구현

#### **features/F01~F06.md** - 기능별 상세 요구사항
- **언제 읽나**: 각 기능 구현 직전 (부트 프로토콜 6단계)
- **목적**: 기능별 유저 스토리, UI 계약, 데이터 규칙, 완성 기준 제시
- **예시**: F01_AddTodo.md → "할일 추가: 3초 이내 완료, 공백 저장 금지, 최상단 입력창"
- **AI 활용**: TodoService.add() 메서드와 UI 구현에 직접 반영

#### **Tasks.md** - 구현 순서
- **언제 읽나**: 실제 코딩 시작 전 계획 수립 (부트 프로토콜 7단계)
- **목적**: 0→7 단계 크리티컬 패스 제시
- **AI 활용**: 이 순서대로 코드 생성 (Domain → Data → ViewModel → UI)

#### **Test.md** - 테스트 기준
- **언제 읽나**: 각 기능 구현 완료 후 테스트 작성 시 (부트 프로토콜 8단계)
- **목적**: Unit/UI/수동 테스트, A11y ID, DoD 기준 제시
- **예시**: "field.input에서 입력 후 btn.add → list.todo 상단에 추가"
- **AI 활용**: UI 테스트 코드 자동 생성

### **🔧 source/core/ - AI 의사결정 시스템**

#### **DECISION_MATRIX.md** - 의사결정 기준
- **언제 읽나**: 구현 중 선택이 필요한 상황 (부트 프로토콜 4단계)
- **목적**: 사용자 경험/성능/안정성/확장성/개발속도 가중치 제시
- **예시 상황**: "입력창을 상단에 둘까, 하단에 둘까?"
- **AI 활용**: "사용자 경험 40%, 성능 35% 가중치" → 상단 배치 결정

#### **ADAPTATION_RULES.md** - 상황별 적응 규칙
- **언제 읽나**: 사용자 입력에 따른 동적 조정 시 (부트 프로토콜 5단계)
- **목적**: IF-THEN 형태의 조건부 기능 추가 규칙
- **예시**: "IF 사진 첨부 요구 THEN ImagePicker + Core Data Binary 추가"
- **AI 활용**: 특별 요구사항에 따른 추가 태스크 자동 생성

#### **QUALITY_GATES.md** - 품질 기준
- **언제 읽나**: 각 단계 완료 시 품질 검증 (부트 프로토콜 9단계)
- **목적**: 성능, 사용성, 안정성, 보안 등 정량적 기준 제시
- **예시**: "빌드 에러 0개, 크래시 0개, 접근성 ID 필수"
- **AI 활용**: 기준 통과할 때까지 자동 수정

#### **VALIDATION_CHECKLIST.md** - 검증 시나리오
- **언제 읽나**: 최종 테스트 시 (부트 프로토콜 10단계)
- **목적**: 사용자 시나리오 기반 종합적 검증
- **예시**: "연속 10건 입력, 다크/라이트 모드, SE~Pro Max 테스트"
- **AI 활용**: 시나리오별 검증 자동 수행

#### **LEARNING_SIGNALS.md** - 성공/실패 지표
- **언제 읽나**: 개발 완료 후 결과 분석
- **목적**: 사용자 채택률, 성능 지표, 피드백 분석
- **AI 활용**: 다음 개발을 위한 학습 데이터 수집

---

## 🏗️ 기술적 구현 구조

### **아키텍처: Clean Architecture + MVVM**
```
Presentation Layer (UI)
├── Views/ (SwiftUI)
│   ├── ContentView.swift - 메인 화면
│   └── TodoRowView.swift - 리스트 아이템
└── ViewModels/ (State 관리)
    └── TodoViewModel.swift - 상태 관리
↓ (단방향 의존성)
Domain Layer (비즈니스 로직)
├── Entities/ (도메인 모델)
│   └── TodoItem.swift - 핵심 엔티티
├── UseCases/ (비즈니스 규칙)
│   └── TodoService.swift - 비즈니스 로직
└── RepositoryInterfaces/ (프로토콜)
    └── TodoRepository.swift - 인터페이스
↓ (단방향 의존성)
Data Layer (데이터 접근)
├── Repositories/ (구현체)
│   └── CoreDataTodoRepository.swift - 실제 구현
└── CoreData/ (영속성)
    ├── TodoModel.xcdatamodeld - 데이터 모델
    └── PersistenceController.swift - Core Data 설정
```

### **개발 순서: Tasks.md 기준 7단계**
```
0. 프로젝트 설정
   - Xcode 프로젝트 생성 확인
   - 폴더 구조 생성
   - 기본 설정 검증

1. 데이터 모델링 (Domain + CoreData)
   - Domain/Entities/TodoItem.swift
   - Domain/RepositoryInterfaces/TodoRepository.swift
   - Data/CoreData/TodoModel.xcdatamodeld
   - Data/PersistenceController.swift

2. 저장소 + 유스케이스
   - Data/Repositories/CoreDataTodoRepository.swift
   - Domain/UseCases/TodoService.swift (add/toggle/delete/edit/stats)

3. ViewModel
   - Presentation/ViewModels/TodoViewModel.swift
   - state: input, todos, stats
   - intents: onAdd, onToggle, onDelete, onEdit, onChangeInput

4. UI (SwiftUI)
   - Presentation/Views/ContentView.swift (상단 입력/중간 통계/하단 리스트)
   - Presentation/Views/TodoRowView.swift (체크박스/스와이프/인라인 편집)

5. 접합/정리
   - 의존성 방향 점검 (Presentation → Domain → Data)
   - 파일 경로/그룹 구조 재검증
   - A11y ID 최종 확인

6. 테스트/품질
   - Unit: Repository, Service
   - UI: 핵심 플로우(추가/토글/삭제/편집), A11y ID 기반
   - 수동 체크리스트: Test.md 기준 전부 체크

7. 산출/리포트
   - stdout에 최종 요약(JSON): 통과/실패, 테스트 개수, 경과시간
```

---

## 📊 각 파트별 요약 정리

### **🎯 1. 환경 검증 파트 (PREFLIGHT)**
**핵심 포인트:**
- 성공적인 개발을 위한 사전 검증 시스템
- A/B/C 모드로 사용자 상황에 맞는 경로 제공
- 실패 가능성을 미리 차단하여 사용자 경험 보장

**사용자를 위한 가이드:**
- A: Xcode 있고 시간 충분 → 완전한 앱 개발
- B: 시간 부족하거나 환경 불확실 → 빠른 체험
- C: 정보만 필요 → 가이드 제공

### **🎯 2. 요구사항 수집 파트 (APP_DISCOVERY)**
**핵심 포인트:**
- 8단계 체계적 질문으로 완전한 맞춤 정보 수집
- 막연한 요청을 구체적인 개발 요구사항으로 변환
- 사용자별 맞춤형 개발의 핵심 기반

**개발자를 위한 가이드:**
- Step 1-2: 앱 종류 확정
- Step 3: 타겟 사용자 명확화
- Step 4-5: 핵심 가치와 특별 요구사항 수집

### **🎯 3. 개발 실행 파트 (CLAUDE_TEMPLATE)**
**핵심 포인트:**
- 11단계 자동 읽기 순서로 완전 자동화
- Clean Architecture + MVVM 전문적 구조
- 품질 게이트 통과 보장 시스템

**AI를 위한 가이드:**
- 반드시 Xcode 프로젝트 생성 후 코드 작성 시작
- 부트 프로토콜 순서 엄격히 준수
- JSON Lines 로그로 진행상황 실시간 추적

### **🎯 4. 커스텀 Task List 생성 (5단계 로직)**
**핵심 포인트:**
- 기존 검증된 구조 + 사용자 맞춤 요구사항 조합
- 동적 태스크 생성으로 완전히 새로운 앱 개발 가능
- 의사결정 규칙 기반 우선순위 자동 조정

**시스템 특징:**
- 1단계: 기본 템플릿 (source/examples/[카테고리]/Tasks.md)
- 2단계: 사용자 요구사항 반영 (02_APP_DISCOVERY.md)
- 3단계: 기능별 세부화 (features/*.md)
- 4단계: 우선순위 조정 (DECISION_MATRIX.md)
- 5단계: 특별 요구사항 추가 (ADAPTATION_RULES.md)

### **🎯 5. AI 의사결정 시스템 (core/)**
**핵심 포인트:**
- AI가 일관되고 합리적인 결정을 내릴 수 있는 기준 제공
- 사용자 맥락 반영한 정량적 의사결정
- 품질과 성능을 보장하는 게이트 시스템

**품질 보장 체계:**
- DECISION_MATRIX: 가중치 기반 선택
- ADAPTATION_RULES: 조건부 기능 추가
- QUALITY_GATES: 정량적 품질 기준
- VALIDATION_CHECKLIST: 종합적 검증

---

## 🚀 결론

**ai_app_template**은 단순한 코드 생성 도구가 아닌, **완전한 iOS 앱 개발 자동화 시스템**입니다.

### **시스템의 강점:**
1. **체계적 요구사항 수집**: 막연한 요청을 구체적 요구사항으로 변환
2. **동적 맞춤화**: 사용자별 완전히 다른 앱 생성 가능
3. **전문적 품질**: Clean Architecture + 테스트 자동 생성
4. **완전 자동화**: 사용자는 질문 답변만, AI가 모든 개발 담당

### **사용자별 활용 방안:**
- **비개발자**: 아이디어만 있으면 완성된 앱 확보
- **개발자**: 빠른 프로토타입 + 전문적 구조 학습
- **기업**: 내부 도구 빠른 개발 + 표준화된 품질

이 시스템을 통해 누구나 전문적인 iOS 앱을 쉽고 빠르게 개발할 수 있습니다.