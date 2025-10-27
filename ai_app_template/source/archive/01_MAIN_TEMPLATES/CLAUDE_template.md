CLAUDE.md — System Contract (READ FIRST)

이 문서를 먼저 읽고 그대로 수행하면, 같은 리포지토리의 나머지 .md 파일들을 스스로 찾아 읽어 {{APP_NAME}} iOS 앱을 구현하고, 테스트까지 마칠 수 있다.
목표는 명세 준수, 재현 가능한 구조, DoD(완료 기준) 충족이다.

⭐ **언어 사용 규칙 (최우선)**:
• 모든 설명, 안내, 에러 메시지는 반드시 한국어로 작성
• 코드 주석도 모두 한국어로 작성  
• 변수명과 함수명은 영어, 설명은 한국어
• 사용자와의 모든 소통은 한국어만 사용

⸻

0) 프리플라이트 검증 시스템 (최우선 실행)

⚡ **모든 앱 개발 요청에 대해 프리플라이트 검증 필수**

🔍 **1단계: 개발 준비도 검증**

**모든 앱 개발 요청 시 반드시 먼저 실행:**
- **즉시 중단**: 이 템플릿을 계속 읽지 말고 대신 다음을 수행
- **PREFLIGHT_CHECK.md 실행**: ../00_PREFLIGHT/PREFLIGHT_CHECK.md 파일 실행
- **3단계 검증 프로세스**: 환경 확인 → 목적 확인 → 경로 결정
- **검증 통과 시만**: 아래 인터랙티브 모드로 진행

🎯 **2단계: 검증 통과 후 모드 분기**

**A. 완전 개발 모드 (검증 통과 시):**
- **APP_DISCOVERY.md 실행**: ../02_INTERACTIVE_DISCOVERY/APP_DISCOVERY.md 실행
- **8단계 인터랙티브 프로세스**: 앱 카테고리 선택 → 세부 타입 → 사용자 프로파일 → 핵심 가치 → 요구사항 → 템플릿 선택 → 최종 확인
- **동적 템플릿 생성**: 수집된 정보로 {{APP_NAME}}, {{CORE_FEATURES}} 등 변수 설정
- **개발 시작**: 모든 정보 수집 완료 후 이 템플릿의 1단계로 돌아와서 계속 진행

**B. 빠른 테스트 모드:**
- **예시 앱 선택**: ../examples/ 폴더에서 적합한 예시 선택
- **코드 구조 생성**: 학습 목적의 간단한 구현

**C. 정보 제공 모드:**
- **가이드 문서 제공**: 개발 환경 설정 및 학습 리소스 안내

**⚠️ 중요: 반드시 프리플라이트 검증을 먼저 완료하고, 검증 결과에 따라 적절한 모드로 진행하세요.**

⸻

1) 인터랙티브 확인 완료 후 단계

⚡ **개발 시작 전 필수 단계** (인터랙티브 완료 후)

{{APP_NAME}} 개발을 시작하기 전에 다음 단계를 완료해주세요:

🚀 **1단계: Xcode 프로젝트 먼저 생성 (필수)**
```
⚠️ 중요: AI가 코드를 생성하기 전에 반드시 Xcode 프로젝트를 먼저 생성해주세요.

Xcode에서 새 프로젝트 생성:
1. Xcode 실행 → "Create New Project" 선택
2. iOS 탭 → "App" 선택 → Next
3. Product Name: {{APP_NAME}} (원하는 정확한 이름으로 입력)
4. Interface: SwiftUI, Language: Swift, Core Data: 체크
5. 저장 위치 선택 후 "Create" 클릭
6. 빌드 확인: ⌘ + B 눌러서 에러 없이 빌드되는지 확인

프로젝트 생성이 완료되면 "프로젝트 생성 완료!"라고 알려주세요.
```

📋 **2단계: 개발 계획 확인**
- **앱 종류**: {{APP_NAME}} (Xcode에서 생성한 정확한 이름 사용)
- **핵심 기능**: {{CORE_FEATURES}}
- **UI 규칙**: {{UI_LAYOUT_RULES}}
- **타겟 사용자**: {{TARGET_USER}}

🎯 **3단계: 최종 확인**
1. Xcode 프로젝트가 정상적으로 생성되었나요?
2. 이 개발 방향이 원하시는 것과 맞나요?
3. 수정하고 싶은 부분이 있나요?

**프로젝트 생성 확인 후 개발을 시작합니다!**

⸻

2) 실행 요약 (사용자 승인 후)
	•	플랫폼: iOS 16.0+, Swift 5.8+, SwiftUI
	•	아키텍처: Presentation → Domain → Data (단방향), MVVM + Clean Architecture
	•	핵심 기능: {{CORE_FEATURES}}
	•	UI 불변: {{UI_LAYOUT_RULES}}

⸻

3) 읽기/실행 순서
	1.	이 문서(CLAUDE.md)를 전부 읽는다.
	2.	USER_CONTEXT.md 를 읽어 사용자 맥락과 의도를 파악한다.
	3.	BUSINESS_RULES.md 를 읽어 사용자별 커스터마이징 요구사항을 수집한다.
	4.	../core/DECISION_MATRIX.md 를 읽어 의사결정 기준과 우선순위를 확정한다.
	5.	../core/ADAPTATION_RULES.md 를 읽어 상황별 적응 규칙을 파악한다.
	6.	features/*.md 을 번호 순서로 모두 읽어 요구사항과 DoD를 수집한다.
	7.	Tasks.md 를 읽어 크리티컬 패스(프로젝트/모델/서비스/UI/테스트) 실행 계획을 확정한다.
	8.	Test.md 를 읽어 자동/수동 테스트, A11y ID, DoD를 고정한다.
	9.	../core/QUALITY_GATES.md 를 읽어 품질 기준을 확정한다.
	10.	../core/VALIDATION_CHECKLIST.md 를 읽어 검증 시나리오를 준비한다.
	12.	아래 부트 프로토콜을 즉시 수행한다.

폴더/파일이 누락되어도 스스로 생성해야 한다. 외부 질문 없이 문서만으로 진행할 수 있게 작성되어 있다.

⸻

4) 에이전트 부트 프로토콜 (필수 절차)

부트 시작 시점부터 모든 주요 단계에서 JSON Lines 로그를 stdout 으로 출력한다(수집용).
	•	예시 JSONL 한 줄:
{"phase":"build","taskId":"BOOT","status":"start","etaSec":120,"details":"read CLAUDE.md"}

	1.	BOOT
	•	현재 문서 파싱 → 규칙/금지/DoD 스냅샷 확보.
	•	리포 루트에서 경로 확인: Tasks.md, Test.md, features/. 없으면 생성(빈 파일 금지).
	•	**프로젝트 감지**: 기존 Xcode 프로젝트 존재 여부 확인 후 실제 프로젝트명 사용.
	2.	PLAN
	•	features/*.md 요구사항을 태스크로 정규화.
	•	Tasks.md 순서에 맞춰 작업 계획을 잠깐 요약 로그로 출력(status:"progress").
	3.	PROJECT SETUP - 사용자 우선 프로젝트 생성 (단순화)
	•	**1단계: 기존 프로젝트 확인**
	  ```bash
	  # 기존 Xcode 프로젝트 감지
	  XCODE_PROJECT=$(find . -name "*.xcodeproj" | head -1)
	  if [ -z "$XCODE_PROJECT" ]; then
	      echo "❌ Xcode 프로젝트가 없습니다."
	      echo "💡 AI_{{APP_NAME}} 폴더에 임시로 파일을 생성합니다."
	      echo "나중에 Xcode 프로젝트 생성 후 FILE_MIGRATION_RULES.md를 참조하여 파일을 이동하세요."
	      export ACTUAL_PROJECT_NAME="AI_{{APP_NAME}}"
	  else
	      ACTUAL_PROJECT_NAME=$(basename "$XCODE_PROJECT" .xcodeproj)
	      echo "✅ 기존 프로젝트 발견: $ACTUAL_PROJECT_NAME"
	      echo "🎯 이 프로젝트명을 기준으로 모든 파일을 생성합니다."
	      export ACTUAL_PROJECT_NAME="$ACTUAL_PROJECT_NAME"
	  fi
	  ```
	  
	•	**2단계: Core Data 검증 (기존 프로젝트가 있는 경우만)**
	  - .xcdatamodeld 파일에서 실제 Entity 이름 확인
	  - 모든 Swift 파일의 Entity 참조 일관성 검사
	  - 중복 Persistence 파일 존재 시 CORE_DATA_ISSUES.md 참조하여 해결
	  
	•	루트 구조와 소스 그룹을 다음과 같이 만든다 ($ACTUAL_PROJECT_NAME 사용):
$ACTUAL_PROJECT_NAME/
├─ Presentation/                # UI 레이어
│  ├─ Views/ (ContentView.swift, {{MAIN_ROW_VIEW}}.swift)
│  └─ ViewModels/ ({{MAIN_VIEW_MODEL}}.swift)
├─ Domain/                      # 비즈니스 규칙
│  ├─ Entities/ ({{MAIN_ENTITY}}.swift)
│  ├─ UseCases/ ({{MAIN_SERVICE}}.swift)
│  └─ RepositoryInterfaces/ ({{MAIN_REPOSITORY}}.swift)   # protocol
├─ Data/                        # 데이터 접근
│  ├─ Repositories/ (CoreData{{MAIN_REPOSITORY}}.swift)
│  └─ CoreData/ ({{DATA_MODEL}}.xcdatamodeld, PersistenceController.swift)
├─ ${ACTUAL_PROJECT_NAME}App.swift         # 앱 엔트리
└─ Assets.xcassets/
	•	의존 방향: Presentation → Domain → Data. 역참조 금지(Protocol로 주입).
	•	Info.plist: 최소 버전 iOS 16.0 설정.
	4.	IMPLEMENT (Tasks.md의 순서 그대로)
	•	Domain 모델/프로토콜 → Data(Core Data) → UseCase(Service) → ViewModel → Views(UI).
	•	기능별 상세는 features/*.md 를 명세 원본으로 삼는다.
	•	사용자 맥락은 USER_CONTEXT.md와 BUSINESS_RULES.md를 기반으로 커스터마이징한다.
	•	구현 중 선택이 필요한 상황에서는 ../core/DECISION_MATRIX.md와 ../core/ADAPTATION_RULES.md를 참조한다.
	•	A11y ID 전역 사용(아래 §4).
	•	비즈니스 로직과 데이터 처리는 Domain 기준으로 일관 유지.
	5.	TEST
	•	Test.md의 Unit/UI/수동 테스트를 모두 구현/수행한다.
	•	../core/QUALITY_GATES.md의 성능/안정성/접근성 기준을 모두 통과해야 한다.
	•	../core/VALIDATION_CHECKLIST.md의 사용자 시나리오 기반 검증을 수행한다.
	•	실패 시 status:"error" 로그와 원인/위치/다음 조치 제안 1가지 이상을 남긴다.
	•	DoD 전부 통과 전에는 "완료" 선언 금지.
	6.	LEARNING & SUMMARY
	•	../core/LEARNING_SIGNALS.md 기준으로 성공/실패 지표를 측정하고 기록한다.
	•	마지막에 요약 JSON 1줄 출력: 총 테스트 수/통과/실패/경과시간/산출물 경로/학습 포인트.

⸻

5) UI 레이아웃 불변 규칙
{{UI_LAYOUT_RULES_DETAILED}}

⸻

6) 접근성(A11y IDs, 전역 고정)
{{A11Y_IDS}}

모든 UI 테스트는 이 A11y ID를 기준으로 작성/실행한다. ID 변경 금지.

⸻

7) 데이터/도메인 규칙 (요지)
{{DATA_DOMAIN_RULES}}

⸻

8) 금지/강제 목록
	•	❌ {{FORBIDDEN_PATTERNS}}
	•	✅ {{REQUIRED_PATTERNS}}
	•	✅ 주요 단계 JSONL 로그 필수(아래 §7)

⸻

9) 진행 로그(JSON Lines) 규격
	•	공통 필드: phase(build|run|test), taskId("T-001" 등), status(start|progress|done|error), etaSec(정확치 않으면 null), details(상황 요약)
	•	예시:
{"phase":"build","taskId":"T-002","status":"progress","etaSec":90,"details":"Implement {{MAIN_SERVICE}}.add/toggle/delete/edit"}
{"phase":"test","taskId":"UI-CORE","status":"done","etaSec":null,"details":"Add/Toggle/Delete/Edit passed (A11y OK)"}

위 포맷을 벗어나지 말 것.

⸻

10) 테스트/DoD 게이트 (요약)

아래는 Test.md의 요지를 게이트로 재기술한 것이다. 완전히 통과해야 최종 완료.
{{TEST_DOD_SUMMARY}}

⸻

11) 구현 순서(핵심 체크리스트)

Tasks.md 를 소스오브트루스로 따른다. 최소 체크:
{{IMPLEMENTATION_CHECKLIST}}

각 단계 진입/완료 시 JSONL 로그를 남긴다.

⸻

12) 오류 처리와 BLOCKER 규칙
	•	불명확/충돌 명세를 만나면 임의추측 금지. 다음 한 줄을 즉시 기록하고 중단:
{"phase":"build","taskId":"BLOCKER","status":"error","etaSec":null,"details":"<문제 요약 및 제안 1안>"}
	•	제안 1안에는 가장 안전한 보수적 해석을 넣는다(예: 입력 검증 강화, UI 위치 유지 등).
	•	BLOCKER 해소 후 같은 단계부터 재개한다.

⸻

13) 품질/스타일(필수 요지)
	•	Swift Concurrency는 본 앱 범위에서 과도 사용 지양(필요 최소한)
	•	확장/재사용을 위해 Repository/UseCase는 프로토콜 우선
	•	View는 상태 최소화와 단방향 데이터 흐름 유지
	•	주석은 "왜(의도/제약)" 중심으로, "무엇/어떻게"는 코드로 드러나게 작성

⸻

14) AI 친화적 문서 구조
	•	USER_CONTEXT.md: 사용자 맥락과 의도 파악 (대상 사용자, 핵심 가치, 특별 요구사항)
	•	BUSINESS_RULES.md: 사용자별 비즈니스 로직 커스터마이징 (분류 체계, 시간 관리, 알림 정책)
	•	../core/DECISION_MATRIX.md: AI 의사결정 기준과 우선순위 (가중치, 트레이드오프 규칙)
	•	../core/ADAPTATION_RULES.md: 사용자 입력 기반 동적 적응 규칙 (IF-THEN 로직)
	•	../core/QUALITY_GATES.md: 정량적 품질 기준 (성능, 사용성, 안정성, 보안)
	•	../core/VALIDATION_CHECKLIST.md: 사용자 시나리오 기반 검증 (핵심/스트레스/엣지 케이스)
	•	../core/LEARNING_SIGNALS.md: AI 학습을 위한 성공/실패 지표 (사용자 채택, 성능, 피드백)

⸻

15) 최종 산출물
	•	동작하는 Xcode 프로젝트({{APP_NAME}}), 위 구조/규칙 완전 준수
	•	테스트 통과 결과와 함께 마지막 요약 JSON 1줄을 stdout 으로 출력

⸻

16) 호출 문구(사용자 1줄 명령)
	•	사용자는 다음 한 문장만 입력해도 된다:
{{INVOCATION_PROMPT}}

이 문장으로 호출되면, 본 CLAUDE.md의 부트 프로토콜을 시작하고, 읽기 순서에 따라 USER_CONTEXT.md, BUSINESS_RULES.md, features/*.md, Tasks.md, Test.md 등을 자동 탐색/실행하여 완성한다.