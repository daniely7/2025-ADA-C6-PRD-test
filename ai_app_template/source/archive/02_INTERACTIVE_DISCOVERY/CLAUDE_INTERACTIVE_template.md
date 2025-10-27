CLAUDE.md — Interactive System Contract (READ FIRST)

이 문서를 먼저 읽고 그대로 수행하면, 사용자 요구사항을 인터랙티브하게 파악하여 맞춤형 {{APP_NAME}} iOS 앱을 구현하고, 테스트까지 마칠 수 있다.
목표는 사용자 발견, 맞춤형 설계, 재현 가능한 구조, DoD(완료 기준) 충족이다.

⭐ **언어 사용 규칙 (최우선)**:
• 모든 설명, 안내, 에러 메시지는 반드시 한국어로 작성
• 코드 주석도 모두 한국어로 작성  
• 변수명과 함수명은 영어, 설명은 한국어
• 사용자와의 모든 소통은 한국어만 사용

⸻

0) 인터랙티브 시작 조건 감지

⚡ **모든 앱 개발 요청에 대해 인터랙티브 모드 활성화** (일관성 보장)

A) **막연한 요청** → 완전 APP_DISCOVERY 모드:
• "ai_app_template을 기반으로 앱을 개발해줘!"
• "앱 만들어줘"  
• "무언가 유용한 앱을 개발하고 싶어"
• 구체적인 앱 종류나 요구사항이 없는 모든 요청

B) **구체적 요청** → 요구사항 확인 + 템플릿 선택 모드:
• "ai_app_template을 기반으로 운동 기록 앱을 개발해줘"
• "가계부 앱 만들어줘"
• "examples/fitness_app 기반으로 개발해줘"
• 특정 앱 종류나 파일을 명시한 모든 요청

C) **예시 파일 직접 지정** → 확인 후 진행 모드:
• "examples/fitness_app 폴더의 CLAUDE.md 파일을 보고 개발해줘"
• 특정 경로의 파일을 직접 지정한 요청

→ 요청 유형에 따라 적절한 인터랙티브 프로세스 실행

⸻

1) 요청 유형별 인터랙티브 프로세스

**A) 막연한 요청 → 완전 APP_DISCOVERY 모드:**

	1.	WELCOME & CATEGORY
	•	../APP_DISCOVERY.md의 Step 1 웰컴 메시지 표시
	•	8개 앱 카테고리 옵션 제시
	•	사용자 선택 대기

	2.	DETAIL TYPE
	•	선택된 카테고리의 세부 앱 타입 옵션 제시
	•	각 타입별 특징과 예시 설명
	•	사용자 최종 앱 종류 확정

	3.	USER PROFILING  
	•	주요 사용자 파악 (연령대, 라이프스타일, 기술 수준, 사용 빈도)
	•	구체적인 사용 시나리오 질문
	•	페르소나 정보 수집

	4.	CORE VALUES
	•	핵심 가치 3가지 우선순위 선택
	•	속도/분석/UX/기능깊이/접근성 카테고리별 옵션 제시
	•	사용자 중요도 순위 확정

	5.	SPECIAL REQUIREMENTS & TEMPLATE SELECTION
	•	특별 요구사항 수집 (고급 기능, 디자인, 데이터, 성능)
	•	적합한 기존 예시 템플릿 제안 (예: examples/fitness_app)
	•	새로 생성 vs 예시 활용 선택권 제공
	•	최종 정보 요약 및 확인
	•	개발 시작 승인

**B) 구체적 요청 → 요구사항 확인 + 템플릿 선택 모드:**

	1.	REQUEST CONFIRMATION
	•	"[구체적 앱 종류] 개발 요청을 확인했습니다!"
	•	현재 파악된 정보 요약 표시
	•	추가로 필요한 정보 식별

	2.	CONTEXT COLLECTION  
	•	주요 사용자 정보 수집 (간소화된 질문)
	•	핵심 기능 우선순위 확인
	•	특별 요구사항 수집

	3.	TEMPLATE SELECTION
	•	관련 기존 예시 템플릿 제안
	•	"기존 완성된 [관련앱] 예시(examples/[app])를 활용하시겠어요?"
	•	"아니면 처음부터 새로 만드시겠어요?"
	•	사용자 선택에 따른 개발 방향 확정

	4.	DEVELOPMENT PLAN APPROVAL
	•	수집된 정보 기반 개발 계획 요약
	•	사용자 최종 승인 요청
	•	개발 시작

**C) 예시 파일 직접 지정 → 확인 후 진행 모드:**

	1.	FILE CONFIRMATION
	•	지정된 파일 경로 확인 및 로딩
	•	해당 예시의 앱 종류와 특징 요약 표시

	2.	CUSTOMIZATION CHECK
	•	"이 예시를 그대로 활용하시겠어요?"
	•	"아니면 일부 수정하고 싶은 부분이 있나요?"
	•	수정 희망사항 수집

	3.	FINAL APPROVAL
	•	개발 계획 확정 및 시작 승인
	•	지정된 예시 기반 개발 진행

⸻

2) 동적 템플릿 생성 (수집된 정보 기반)

	수집 완료 후 다음 정보로 동적 커스터마이징:
	•	{{APP_NAME}} = 앱 종류 기반 자동 생성, PascalCase 필수 (예: FitnessTracker, MoneyManager, OnePageTodo)
	•	{{CORE_FEATURES}} = 핵심 가치 기반 기능 매핑
	•	{{MAIN_ENTITY}} = 앱 타입별 주요 엔티티 (WorkoutRecord, Transaction, TodoItem)
	•	{{UI_LAYOUT_RULES}} = 사용자 프로필 기반 UI 규칙
	•	{{TARGET_USER}} = 수집된 사용자 정보
	•	{{PRIORITY_VALUES}} = 핵심 가치 우선순위

⸻

3) 적응형 예시 선택

	수집된 정보를 바탕으로 가장 적합한 기본 템플릿 선택:
	•	생산성 앱 → examples/todo_app 기반 확장
	•	피트니스 앱 → examples/fitness_app 활용
	•	금융 앱 → examples/finance_app 활용
	•	학습/취미 → examples/reading_app 기반
	•	기타 → 가장 유사한 예시를 기반으로 커스터마이징

⸻

4) 실행 요약 (동적 생성)

	•	플랫폼: iOS 16.0+, Swift 5.8+, SwiftUI
	•	아키텍처: Presentation → Domain → Data (단방향), MVVM + Clean Architecture
	•	핵심 기능: {{CORE_FEATURES}} (사용자 맞춤)
	•	UI 불변: {{UI_LAYOUT_RULES}} (사용자 프로필 기반)
	•	타겟 사용자: {{TARGET_USER}}

⸻

5) 읽기/실행 순서 (기존과 동일)

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

⸻

6) 에이전트 부트 프로토콜 (인터랙티브 확장)

부트 시작 시점부터 모든 주요 단계에서 JSON Lines 로그를 stdout 으로 출력한다(수집용).

	1.	DISCOVERY (모든 요청에 적용)
	•	요청 유형 분석 (막연함/구체적/파일지정)
	•	해당 유형에 맞는 인터랙티브 프로세스 실행
	•	A) 막연한 요청 → 완전 APP_DISCOVERY 모드
	•	B) 구체적 요청 → 요구사항 확인 + 템플릿 선택
	•	C) 파일 지정 → 확인 후 진행
	•	단계별 사용자 응답 수집 및 로깅
	•	동적 템플릿 변수 생성 및 적용

	2.	BOOT
	•	현재 문서 파싱 → 규칙/금지/DoD 스냅샷 확보
	•	동적 생성된 변수들로 템플릿 커스터마이징
	•	리포 루트에서 경로 확인: Tasks.md, Test.md, features/. 없으면 생성(빈 파일 금지)

	3.	PLAN
	•	features/*.md 요구사항을 태스크로 정규화
	•	사용자 우선순위에 따른 기능 순서 조정
	•	Tasks.md 순서에 맞춰 작업 계획을 잠깐 요약 로그로 출력(status:"progress")

	4.	CREATE PROJECT (기존과 동일)
	•	사용자 주도 Xcode 프로젝트 생성 안내
	•	파일 이동 및 Core Data 검증
	•	프로젝트 구조 설정

	5.	IMPLEMENT (적응형)
	•	사용자 핵심 가치 우선순위에 따른 구현 순서
	•	Domain 모델/프로토콜 → Data(Core Data) → UseCase(Service) → ViewModel → Views(UI)
	•	각 단계에서 사용자 피드백 반영

⸻

7) 사용자 피드백 루프

	각 주요 단계 완료 후 사용자 확인 요청:
	•	"이 방향이 맞나요? 수정하고 싶은 부분이 있나요?"
	•	"더 중요하게 생각하는 기능이 있나요?"
	•	"UI가 원하시는 스타일인가요?"

	피드백 기반 실시간 조정:
	•	기능 우선순위 재조정
	•	UI 스타일 변경
	•	추가 요구사항 반영

⸻

8) 완료 및 최적화

	기본 구현 완료 후:
	•	사용자 테스트 피드백 수집
	•	성능 최적화 제안
	•	추가 기능 개발 여부 확인
	•	배포 및 유지보수 가이드 제공

⸻

⚠️ 중요 사항:
• 사용자 응답 없이는 다음 단계로 진행하지 않음
• 모든 선택사항은 사용자 확인 후 적용
• 개발 중에도 언제든 방향 수정 가능
• 사용자 만족도를 최우선으로 개발 진행

이 인터랙티브 템플릿으로 어떤 막연한 요청도 완벽한 맞춤형 앱으로 변환할 수 있다.