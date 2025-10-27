# 🤖 AI App Development Framework

> AI가 사용자 맥락을 이해하고 맞춤형 iOS 앱을 개발할 수 있도록 하는 완전한 프레임워크

## 🎯 Quick Start - 3가지 시작 방법

> 💡 **가이드 파일 설명**:
> - **하나의 프롬프트로 모든 앱 개발**: 이제 복잡한 설정 없이 바로 시작 (모든 사용자)
> - **[PROMPTS_GUIDE.md](./PROMPTS_GUIDE.md)**: 모든 종류의 앱 개발 가이드 (고급 사용자)

### 🚀 메인 프롬프트 (모든 상황에 사용 가능) ⏱️ 30분-3시간
**언제 사용?** 처음 사용, 투두리스트 앱, 피트니스 앱, 가계부 앱 등 **모든 앱 개발**

**상세 가이드:** [PROMPTS_GUIDE.md](./PROMPTS_GUIDE.md) 파일 참고

```bash
"ai_app_template을 기반으로 앱을 개발해줘!

**중요: 모든 설명과 안내는 한국어로 해줘.**"
```

**🎯 이 프롬프트가 제공하는 3가지 자동 경로:**
- **완전 개발 모드**: Xcode 환경에서 실제 동작하는 앱 완성 (2-3시간)
- **빠른 테스트 모드**: 코드 구조 체험 및 학습 (30분-1시간)  
- **정보 제공 모드**: 환경 설정 가이드 및 학습 리소스

### 🎨 고급 사용자 (선택적) ⏱️ 다양
**언제 사용?** 특정 예시 앱을 직접 지정하거나, 아키텍처를 수정할 때

```bash
"ai_app_template/examples/todo_app 폴더의 CLAUDE.md 파일을 보고 iOS앱을 개발해줘!

**중요: 모든 설명과 안내는 한국어로 해줘.**"
```

---

## 📁 폴더 구조

```
ai_app_template/               # 🎯 완전한 AI 앱 개발 프레임워크
├── core/                     # 모든 앱 공통 문서 (5개)
│   ├── DECISION_MATRIX.md         # AI 의사결정 기준
│   ├── ADAPTATION_RULES.md        # 동적 적응 규칙
│   ├── QUALITY_GATES.md           # 품질 기준
│   ├── VALIDATION_CHECKLIST.md    # 검증 체크리스트
│   └── LEARNING_SIGNALS.md        # 학습 신호
├── templates/                # 앱별 커스터마이징 템플릿 (5개)
│   ├── CLAUDE_template.md         # 메인 지침서 템플릿
│   ├── USER_CONTEXT_template.md   # 사용자 맥락 템플릿
│   ├── BUSINESS_RULES_template.md # 비즈니스 규칙 템플릿
│   ├── Tasks_template.md          # 구현 순서 템플릿
│   └── Test_template.md           # 테스트 전략 템플릿
├── examples/                 # 완성된 앱 예시
│   └── todo_app/            # OnePageTodo 완성 예시
│       ├── CLAUDE.md
│       ├── USER_CONTEXT.md
│       ├── BUSINESS_RULES.md
│       ├── features/
│       └── [전체 프레임워크 파일들]
├── README.md                 # 🚀 이 파일 (프레임워크 가이드)
├── PROMPTS_GUIDE.md         # 📝 모든 앱 개발 프롬프트 가이드
├── 00_PREFLIGHT/            # 🔍 개발 전 환경 검증 시스템
├── 01_MAIN_TEMPLATES/       # 📋 메인 개발 템플릿
└── 02_INTERACTIVE_DISCOVERY/ # 🎯 인터랙티브 앱 발견 시스템
```

---

## 🎨 지원되는 앱 타입

### 📋 생산성 앱
- **할 일 관리**: OnePageTodo (완성 예시)
- **메모/노트**: 빠른 메모, 마크다운 지원
- **시간 관리**: 포모도로, 시간 추적
- **습관 관리**: 습관 체크, 연속 기록

### 💪 건강/피트니스 앱  
- **운동 기록**: 세트/무게/시간 기록
- **식단 관리**: 칼로리, 영양소 추적
- **수면 추적**: 수면 패턴 기록
- **체중 관리**: 체중 변화 추적

### 💰 금융/가계부 앱
- **가계부**: 수입/지출 기록
- **예산 관리**: 카테고리별 예산 설정
- **투자 추적**: 포트폴리오 관리
- **목표 저축**: 저축 목표 달성

### 📚 학습/취미 앱
- **독서 관리**: 읽은 책, 리뷰 기록
- **언어 학습**: 단어장, 진도 추적
- **취미 기록**: 요리, 여행, 사진 등
- **프로젝트 관리**: 개인 프로젝트 진행

---

## 🔧 새로운 앱 개발 프로세스

### 1️⃣ 프로젝트 준비
```bash
# 1. 새 프로젝트 폴더 생성
mkdir my_new_app
cd my_new_app

# 2. 템플릿 복사
cp -r ai_app_template/templates/* ./
cp -r ai_app_template/examples/[similar_app_type]/* ./  # 선택사항
```

### 2️⃣ 템플릿 커스터마이징

**필수 수정 파일:**
1. **USER_CONTEXT.md** ⭐️ - 사용자 맥락 정의
2. **BUSINESS_RULES.md** ⭐️ - 비즈니스 로직 정의  
3. **CLAUDE_template.md → CLAUDE.md** ⭐️ - 변수 치환
4. **Tasks_template.md → Tasks.md** - 구현 순서 조정
5. **Test_template.md → Test.md** - 테스트 전략 수정

**변수 치환 예시:**
```markdown
# Before (template)
{{APP_NAME}} → FitnessTracker (PascalCase 필수)
{{MAIN_ENTITY}} → WorkoutRecord  
{{CORE_FEATURES}} → 운동 기록(F01), 진행률 추적(F02), 통계 조회(F03)

# After (customized)
FitnessTracker (반드시 PascalCase로 유지)
WorkoutRecord
운동 기록(F01), 진행률 추적(F02), 통계 조회(F03)
```

### 3️⃣ 기능 명세 작성
```bash
mkdir features
# F01_RecordWorkout.md
# F02_ViewProgress.md  
# F03_Statistics.md
# 등등...
```

### 4️⃣ AI 개발 실행
```bash
"이 폴더의 CLAUDE.md 파일을 보고 iOS앱을 개발해줘!"
```

---

## 📊 AI 성능 최적화 특징

### 🎯 사용자 맥락 이해
- **USER_CONTEXT.md**: 대상 사용자, 사용 환경, 핵심 가치 파악
- **BUSINESS_RULES.md**: 비즈니스 로직 커스터마이징
- **ADAPTATION_RULES.md**: 실시간 적응 규칙 (IF-THEN 로직)

### ⚡ 일관된 의사결정
- **DECISION_MATRIX.md**: 우선순위 가중치, 트레이드오프 기준
- **QUALITY_GATES.md**: 정량적 품질 기준 (성능, 안정성, 접근성)
- **VALIDATION_CHECKLIST.md**: 사용자 시나리오 기반 검증

### 🔄 지속적 학습
- **LEARNING_SIGNALS.md**: 성공/실패 지표 수집

---

## 🎉 성공 사례

### OnePageTodo (완성 예시)
- **개발 시간**: AI가 2시간 내 완성
- **코드 품질**: Clean Architecture + MVVM 패턴
- **테스트 커버리지**: Unit + UI + Manual 테스트 100% 통과
- **사용자 피드백**: 직관적이고 빠른 인터페이스

**특징:**
- 상단 고정 입력창으로 빠른 할 일 추가
- 실시간 통계로 진행률 확인
- VoiceOver 100% 지원으로 접근성 우수
- Core Data 기반 안정적인 데이터 저장

---

## 🤝 기여 가이드

### 새로운 앱 타입 예시 추가
```bash
# 1. examples 폴더에 새 타입 추가
mkdir ai_app_template/examples/new_app_type

# 2. 커스터마이징된 템플릿 파일들 추가
# - USER_CONTEXT.md (해당 앱 타입 맞춤)
# - BUSINESS_RULES.md (특화 비즈니스 규칙)
# - features/ (기본 기능 명세들)

# 3. README 업데이트
```

### 템플릿 개선
- 더 나은 변수 구조 제안
- AI 성능 개선을 위한 프롬프트 최적화
- 새로운 core 문서 추가 (필요시)

---

## 📞 문의 및 지원

- **Issues**: GitHub Issues를 통한 버그 리포트
- **Feature Request**: 새로운 앱 타입이나 기능 제안
- **Discussion**: AI 개발 경험 공유

**Happy AI Coding! 🚀**