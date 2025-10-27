CLAUDE.md — System Contract (READ FIRST)

이 문서를 먼저 읽고 그대로 수행하면, 같은 리포지토리의 나머지 .md 파일들을 스스로 찾아 읽어 FitnessTracker iOS 앱을 구현하고, 테스트까지 마칠 수 있다.
목표는 명세 준수, 재현 가능한 구조, DoD(완료 기준) 충족이다.

⭐ **언어 사용 규칙 (최우선)**:
• 모든 설명, 안내, 에러 메시지는 반드시 한국어로 작성
• 코드 주석도 모두 한국어로 작성  
• 변수명과 함수명은 영어, 설명은 한국어
• 사용자와의 모든 소통은 한국어만 사용

⸻

0) 개발 시작 전 필수 단계

⚡ **1단계: Xcode 프로젝트 먼저 생성 (필수)**
```
⚠️ 중요: AI가 코드를 생성하기 전에 반드시 Xcode 프로젝트를 먼저 생성해주세요.

Xcode에서 새 프로젝트 생성:
1. Xcode 실행 → "Create New Project" 선택
2. iOS 탭 → "App" 선택 → Next
3. Product Name: FitnessTracker (원하는 정확한 이름으로 입력)
4. Interface: SwiftUI, Language: Swift, Core Data: 체크
5. 저장 위치 선택 후 "Create" 클릭
6. 빌드 확인: ⌘ + B 눌러서 에러 없이 빌드되는지 확인

프로젝트 생성이 완료되면 "프로젝트 생성 완료!"라고 알려주세요.
```

📋 **2단계: 실행 요약**
	•	플랫폼: iOS 16.0+, Swift 5.8+, SwiftUI
	•	아키텍처: Presentation → Domain → Data (단방향), MVVM + Clean Architecture
	•	핵심 기능: 운동 기록(F01), 운동 루틴 관리(F02), 진행률 추적(F03), 통계 조회(F04)
	•	UI 불변: 헬스장에서 한 손 조작 가능, 빠른 입력 중심, 수치 입력 최적화

⸻

1) 읽기/실행 순서
	1.	이 문서(CLAUDE.md)를 전부 읽는다.
	2.	USER_CONTEXT.md 를 읽어 사용자 맥락과 의도를 파악한다.
	3.	BUSINESS_RULES.md 를 읽어 사용자별 커스터마이징 요구사항을 수집한다.
	4.	../../core/DECISION_MATRIX.md 를 읽어 의사결정 기준과 우선순위를 확정한다.
	5.	../../core/ADAPTATION_RULES.md 를 읽어 상황별 적응 규칙을 파악한다.
	6.	features/*.md 을 번호 순서로 모두 읽어 요구사항과 DoD를 수집한다.
	7.	Tasks.md 를 읽어 크리티컬 패스(프로젝트/모델/서비스/UI/테스트) 실행 계획을 확정한다.
	8.	Test.md 를 읽어 자동/수동 테스트, A11y ID, DoD를 고정한다.
	9.	../../core/QUALITY_GATES.md 를 읽어 품질 기준을 확정한다.
	10.	../../core/VALIDATION_CHECKLIST.md 를 읽어 검증 시나리오를 준비한다.
	12.	아래 부트 프로토콜을 즉시 수행한다.

폴더/파일이 누락되어도 스스로 생성해야 한다. 외부 질문 없이 문서만으로 진행할 수 있게 작성되어 있다.

⸻

2) 에이전트 부트 프로토콜 (필수 절차)

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
	      echo "💡 AI_FitnessTracker 폴더에 임시로 파일을 생성합니다."
	      echo "나중에 Xcode 프로젝트 생성 후 FILE_MIGRATION_RULES.md를 참조하여 파일을 이동하세요."
	      export ACTUAL_PROJECT_NAME="AI_FitnessTracker"
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
	  
	•	루트 구조와 소스 그룹을 다음과 같이 만든다 ($ACTUAL_PROJECT_NAME 사용):
$ACTUAL_PROJECT_NAME/
├─ Presentation/                # UI 레이어
│  ├─ Views/ (ContentView.swift, WorkoutRecordRowView.swift)
│  └─ ViewModels/ (WorkoutViewModel.swift)
├─ Domain/                      # 비즈니스 규칙
│  ├─ Entities/ (WorkoutRecord.swift)
│  ├─ UseCases/ (WorkoutService.swift)
│  └─ RepositoryInterfaces/ (WorkoutRepository.swift)   # protocol
├─ Data/                        # 데이터 접근
│  ├─ Repositories/ (CoreDataWorkoutRepository.swift)
│  └─ CoreData/ (FitnessModel.xcdatamodeld, PersistenceController.swift)
├─ ${ACTUAL_PROJECT_NAME}App.swift      # 앱 엔트리
└─ Assets.xcassets/
	•	의존 방향: Presentation → Domain → Data. 역참조 금지(Protocol로 주입).
	•	Info.plist: 최소 버전 iOS 16.0 설정.
	4.	IMPLEMENT (Tasks.md의 순서 그대로)
	•	Domain 모델/프로토콜 → Data(Core Data) → UseCase(Service) → ViewModel → Views(UI).
	•	기능별 상세는 features/*.md 를 명세 원본으로 삼는다.
	•	사용자 맥락은 USER_CONTEXT.md와 BUSINESS_RULES.md를 기반으로 커스터마이징한다.
	•	구현 중 선택이 필요한 상황에서는 ../../core/DECISION_MATRIX.md와 ../../core/ADAPTATION_RULES.md를 참조한다.
	•	A11y ID 전역 사용(아래 §4).
	•	비즈니스 로직과 데이터 처리는 Domain 기준으로 일관 유지.
	5.	TEST
	•	Test.md의 Unit/UI/수동 테스트를 모두 구현/수행한다.
	•	../../core/QUALITY_GATES.md의 성능/안정성/접근성 기준을 모두 통과해야 한다.
	•	../../core/VALIDATION_CHECKLIST.md의 사용자 시나리오 기반 검증을 수행한다.
	•	실패 시 status:"error" 로그와 원인/위치/다음 조치 제안 1가지 이상을 남긴다.
	•	DoD 전부 통과 전에는 "완료" 선언 금지.
	6.	LEARNING & SUMMARY
	•	../../core/LEARNING_SIGNALS.md 기준으로 성공/실패 지표를 측정하고 기록한다.
	•	마지막에 요약 JSON 1줄 출력: 총 테스트 수/통과/실패/경과시간/산출물 경로/학습 포인트.

⸻

3) UI 레이아웃 불변 규칙
• **한 손 조작 최적화**: 모든 주요 버튼은 화면 하단 1/3 영역에 배치
• **빠른 입력 중심**: 세트/무게/횟수 입력을 위한 숫자 키패드 우선 제공
• **시각적 피드백**: 세트 완료 시 즉시 시각적 확인 (체크마크, 색상 변경)
• **진행 상태 표시**: 현재 운동의 총 세트 중 몇 번째인지 명확히 표시
• **스와이프 제스처**: 세트 삭제는 좌측 스와이프로 통일
• **큰 터치 영역**: 최소 44pt x 44pt 터치 영역 보장 (헬스장 장갑 착용 고려)

⸻

4) 접근성(A11y IDs, 전역 고정)
• addWorkoutButton: "운동 추가 버튼"
• workoutList: "운동 목록"
• workoutRow: "운동 기록 행"
• setInput: "세트 입력 필드"
• weightInput: "무게 입력 필드"
• repsInput: "횟수 입력 필드"
• completeSetButton: "세트 완료 버튼"
• deleteSetButton: "세트 삭제 버튼"
• progressChart: "진행률 차트"
• statisticsView: "통계 화면"

모든 UI 테스트는 이 A11y ID를 기준으로 작성/실행한다. ID 변경 금지.

⸻

5) 데이터/도메인 규칙 (요지)
• **WorkoutRecord**: 운동명, 날짜, 세트 배열, 총 무게/볼륨 계산 로직 포함
• **Set**: 무게(kg), 횟수, 완료 상태를 포함하는 값 타입
• **운동 분류**: 상체/하체/전신으로 자동 분류, 사용자 커스터마이징 허용
• **진행률 계산**: 주간/월간 총 볼륨, 최고 무게, 운동 빈도 기반
• **데이터 검증**: 무게는 0-500kg, 횟수는 1-100 범위로 제한

⸻

6) 금지/강제 목록
	•	❌ 복잡한 운동 분석 알고리즘, 외부 API 연동, 소셜 기능
	•	❌ 타이머나 스톱워치 기능 (빠른 기록에 집중)
	•	❌ 사진이나 동영상 업로드 기능
	•	✅ 빠른 숫자 입력을 위한 커스텀 키패드
	•	✅ 오프라인 우선 설계 (네트워크 불필요)
	•	✅ 주요 단계 JSONL 로그 필수(아래 §7)

⸻

7) 진행 로그(JSON Lines) 규격
	•	공통 필드: phase(build|run|test), taskId("T-001" 등), status(start|progress|done|error), etaSec(정확치 않으면 null), details(상황 요약)
	•	예시:
{"phase":"build","taskId":"T-002","status":"progress","etaSec":90,"details":"Implement WorkoutService.add/complete/delete"}
{"phase":"test","taskId":"UI-CORE","status":"done","etaSec":null,"details":"Add/Complete/Delete/Edit passed (A11y OK)"}

위 포맷을 벗어나지 말 것.

⸻

8) 테스트/DoD 게이트 (요약)

아래는 Test.md의 요지를 게이트로 재기술한 것이다. 완전히 통과해야 최종 완료.
• **Unit 테스트**: WorkoutService CRUD, 볼륨 계산, 진행률 산출 로직
• **UI 테스트**: 운동 추가/완료/삭제 시나리오, A11y ID 기반 자동화
• **수동 테스트**: 한 손 조작, 빠른 입력, 세트 간 전환 시나리오
• **성능 테스트**: 1000개 운동 기록 로딩 2초 이내
• **접근성 테스트**: VoiceOver 100% 호환성

⸻

9) 구현 순서(핵심 체크리스트)

Tasks.md 를 소스오브트루스로 따른다. 최소 체크:
1. Domain 모델 (WorkoutRecord, Set) 정의
2. Core Data 엔티티 매핑 및 Repository 구현
3. WorkoutService 비즈니스 로직 구현
4. WorkoutViewModel 상태 관리 구현
5. 메인 화면 UI (운동 목록, 추가) 구현
6. 세트 입력 UI 및 빠른 입력 최적화
7. 진행률 및 통계 화면 구현
8. 전체 테스트 및 성능 검증

각 단계 진입/완료 시 JSONL 로그를 남긴다.

⸻

10) 오류 처리와 BLOCKER 규칙
	•	불명확/충돌 명세를 만나면 임의추측 금지. 다음 한 줄을 즉시 기록하고 중단:
{"phase":"build","taskId":"BLOCKER","status":"error","etaSec":null,"details":"<문제 요약 및 제안 1안>"}
	•	제안 1안에는 가장 안전한 보수적 해석을 넣는다(예: 입력 검증 강화, UI 위치 유지 등).
	•	BLOCKER 해소 후 같은 단계부터 재개한다.

⸻

11) 품질/스타일(필수 요지)
	•	Swift Concurrency는 본 앱 범위에서 과도 사용 지양(필요 최소한)
	•	확장/재사용을 위해 Repository/UseCase는 프로토콜 우선
	•	View는 상태 최소화와 단방향 데이터 흐름 유지
	•	주석은 "왜(의도/제약)" 중심으로, "무엇/어떻게"는 코드로 드러나게 작성

⸻

12) AI 친화적 문서 구조
	•	USER_CONTEXT.md: 사용자 맥락과 의도 파악 (헬스장 이용자, 빠른 기록 중시, 진행률 확인)
	•	BUSINESS_RULES.md: 운동별 비즈니스 로직 커스터마이징 (분류 체계, 볼륨 계산, 진행률 기준)
	•	../../core/DECISION_MATRIX.md: AI 의사결정 기준과 우선순위 (빠른 입력 vs 정확성, 기능 vs 단순함)
	•	../../core/ADAPTATION_RULES.md: 사용자 입력 기반 동적 적응 규칙 (운동 종류별 가중치 조정)
	•	../../core/QUALITY_GATES.md: 정량적 품질 기준 (성능, 사용성, 안정성, 정확성)
	•	../../core/VALIDATION_CHECKLIST.md: 헬스장 사용 시나리오 기반 검증
	•	../../core/LEARNING_SIGNALS.md: AI 학습을 위한 성공/실패 지표

⸻

13) 최종 산출물
	•	동작하는 Xcode 프로젝트(FitnessTracker), 위 구조/규칙 완전 준수
	•	테스트 통과 결과와 함께 마지막 요약 JSON 1줄을 stdout 으로 출력

⸻

14) 호출 문구(사용자 1줄 명령)
	•	사용자는 다음 한 문장만 입력해도 된다:
"ai_app_template/examples/fitness_app 폴더의 CLAUDE.md 파일을 보고 iOS앱을 개발해줘! 모든 설명과 안내는 한국어로 해줘."

이 문장으로 호출되면, 본 CLAUDE.md의 부트 프로토콜을 시작하고, 읽기 순서에 따라 USER_CONTEXT.md, BUSINESS_RULES.md, features/*.md, Tasks.md, Test.md 등을 자동 탐색/실행하여 완성한다.