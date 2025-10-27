CLAUDE.md — System Contract (READ FIRST)

이 문서를 먼저 읽고 그대로 수행하면, 같은 리포지토리의 나머지 .md 파일들을 스스로 찾아 읽어 원페이지 투두 iOS 앱(OnePageTodo) 을 구현하고, 테스트까지 마칠 수 있다.
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
3. Product Name: OnePageTodo (원하는 정확한 이름으로 입력)
4. Interface: SwiftUI, Language: Swift, Core Data: 체크
5. 저장 위치 선택 후 "Create" 클릭
6. 빌드 확인: ⌘ + B 눌러서 에러 없이 빌드되는지 확인

프로젝트 생성이 완료되면 "프로젝트 생성 완료!"라고 알려주세요.
```

📋 **2단계: 실행 요약**
	•	플랫폼: iOS 16.0+, Swift 5.8+, SwiftUI
	•	아키텍처: Presentation → Domain → Data (단방향), MVVM + Clean Architecture
	•	핵심 기능: 추가(F01), 완료 토글(F02), 삭제(F03), 편집(F04), 영속(F05), 통계(F06)
	•	UI 불변: 입력창은 항상 화면 최상단(고정). 입력 → 통계 → 리스트 순서.

⸻

1) 읽기/실행 순서
	1.	이 문서(CLAUDE.md)를 전부 읽는다.
	2.	USER_CONTEXT.md 를 읽어 사용자 맥락과 의도를 파악한다.
	3.	BUSINESS_RULES.md 를 읽어 사용자별 커스터마이징 요구사항을 수집한다.
	4.	DECISION_MATRIX.md 를 읽어 의사결정 기준과 우선순위를 확정한다.
	5.	ADAPTATION_RULES.md 를 읽어 상황별 적응 규칙을 파악한다.
	6.	features/*.md (F01~F06) 을 번호 순서로 모두 읽어 요구사항과 DoD를 수집한다.
	7.	Tasks.md 를 읽어 크리티컬 패스(프로젝트/모델/서비스/UI/테스트) 실행 계획을 확정한다.
	8.	Test.md 를 읽어 자동/수동 테스트, A11y ID, DoD를 고정한다.
	9.	QUALITY_GATES.md 를 읽어 품질 기준을 확정한다.
	10.	VALIDATION_CHECKLIST.md 를 읽어 검증 시나리오를 준비한다.
	11.	아래 부트 프로토콜을 즉시 수행한다.

폴더/파일이 누락되어도 스스로 생성해야 한다. 외부 질문 없이 문서만으로 진행할 수 있게 작성되어 있다.

⸻

2) 에이전트 부트 프로토콜 (필수 절차)

부트 시작 시점부터 모든 주요 단계에서 JSON Lines 로그를 stdout 으로 출력한다(수집용).
	•	예시 JSONL 한 줄:
{“phase”:“build”,“taskId”:“BOOT”,“status”:“start”,“etaSec”:120,“details”:“read CLAUDE.md”}

	1.	BOOT
	•	현재 문서 파싱 → 규칙/금지/DoD 스냅샷 확보.
	•	리포 루트에서 경로 확인: Tasks.md, Test.md, features/. 없으면 생성(빈 파일 금지).
	•	**프로젝트 감지**: 기존 Xcode 프로젝트 존재 여부 확인 후 실제 프로젝트명 사용.
	2.	PLAN
	•	features/*.md 요구사항을 태스크로 정규화(추가/토글/삭제/편집/영속/통계).
	•	Tasks.md 순서에 맞춰 작업 계획을 잠깐 요약 로그로 출력(status:"progress").
	3.	PROJECT SETUP - 사용자 우선 프로젝트 생성 (단순화)
	•	**1단계: 기존 프로젝트 확인**
	  ```bash
	  # 기존 Xcode 프로젝트 감지
	  XCODE_PROJECT=$(find . -name "*.xcodeproj" | head -1)
	  if [ -z "$XCODE_PROJECT" ]; then
	      echo "❌ Xcode 프로젝트가 없습니다."
	      echo "💡 AI_OnePageTodo 폴더에 임시로 파일을 생성합니다."
	      echo "나중에 Xcode 프로젝트 생성 후 FILE_MIGRATION_RULES.md를 참조하여 파일을 이동하세요."
	      export ACTUAL_PROJECT_NAME="AI_OnePageTodo"
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
│  ├─ Views/ (ContentView.swift, TodoRowView.swift)
│  └─ ViewModels/ (TodoViewModel.swift)
├─ Domain/                      # 비즈니스 규칙
│  ├─ Entities/ (TodoItem.swift)
│  ├─ UseCases/ (TodoService.swift)
│  └─ RepositoryInterfaces/ (TodoRepository.swift)   # protocol
├─ Data/                        # 데이터 접근
│  ├─ Repositories/ (CoreDataTodoRepository.swift)
│  └─ CoreData/ (TodoModel.xcdatamodeld, PersistenceController.swift)
├─ ${ACTUAL_PROJECT_NAME}App.swift         # 앱 엔트리
└─ Assets.xcassets/
	•	의존 방향: Presentation → Domain → Data. 역참조 금지(Protocol로 주입).
	•	Info.plist: 최소 버전 iOS 16.0 설정.
	4.	IMPLEMENT (Tasks.md의 0→7 순서 그대로)
	•	Domain 모델/프로토콜 → Data(Core Data) → UseCase(Service) → ViewModel → Views(UI).
	•	기능별 상세는 features/*.md 를 명세 원본으로 삼는다.
	•	사용자 맥락은 USER_CONTEXT.md와 BUSINESS_RULES.md를 기반으로 커스터마이징한다.
	•	구현 중 선택이 필요한 상황에서는 DECISION_MATRIX.md와 ADAPTATION_RULES.md를 참조한다.
	•	A11y ID 전역 사용(아래 §4).
	•	저장/토글/삭제/편집 로직과 정렬/통계는 Domain 기준으로 일관 유지.
	5.	TEST
	•	Test.md의 Unit/UI/수동 테스트를 모두 구현/수행한다.
	•	QUALITY_GATES.md의 성능/안정성/접근성 기준을 모두 통과해야 한다.
	•	VALIDATION_CHECKLIST.md의 사용자 시나리오 기반 검증을 수행한다.
	•	실패 시 status:"error" 로그와 원인/위치/다음 조치 제안 1가지 이상을 남긴다.
	•	DoD 전부 통과 전에는 "완료" 선언 금지.
	6.	LEARNING & SUMMARY
	•	LEARNING_SIGNALS.md 기준으로 성공/실패 지표를 측정하고 기록한다.
	•	마지막에 요약 JSON 1줄 출력: 총 테스트 수/통과/실패/경과시간/산출물 경로/학습 포인트.

⸻

3) UI 레이아웃 불변 규칙
	•	상단 고정 섹션 순서(위→아래):
	1.	inputSection  2) statisticsSection  3) todoListSection
	•	입력창을 하단 배치하는 구현은 금지.
	•	리스트는 최신 생성순(createdAt 내림차순).
	•	편집은 인라인 전환(Text → TextField)로 수행, 공백 저장 금지.

⸻

4) 접근성(A11y IDs, 전역 고정)
	•	field.input, btn.add, list.todo, row.todo, row.checkbox, btn.delete, btn.edit

모든 UI 테스트는 이 A11y ID를 기준으로 작성/실행한다. ID 변경 금지.

⸻

5) 데이터/도메인 규칙 (요지)
	•	엔티티 TodoItem: id(UUID), title(String), isCompleted(Bool), createdAt(Date), updatedAt(Date)
	•	생성: title 트림 후 공백이면 거부. createdAt=now, updatedAt=createdAt, isCompleted=false
	•	토글: isCompleted 반전, updatedAt=now (정렬은 생성시각 기준 유지)
	•	편집: 트림 후 공백 금지, 성공 시 title/updatedAt 갱신
	•	삭제: 영구 삭제(복구 없음)
	•	통계: total/completed/active, rate=floor(completed/total*100) (total=0 → 0%)

⸻

6) 금지/강제 목록
	•	❌ 입력창 하단 배치
	•	❌ 레이어 역참조(Presentation가 Data 직접 참조 금지)
	•	❌ 루트에 파일 평탄화(지정 폴더 외 생성 금지)
	•	✅ Test.md의 DoD 전부 통과 전 “완료” 선언 금지
	•	✅ 주요 단계 JSONL 로그 필수(아래 §7)

⸻

7) 진행 로그(JSON Lines) 규격
	•	공통 필드: phase(build|run|test), taskId(“T-001” 등), status(start|progress|done|error), etaSec(정확치 않으면 null), details(상황 요약)
	•	예시:
{“phase”:“build”,“taskId”:“T-002”,“status”:“progress”,“etaSec”:90,“details”:“Implement TodoService.add/toggle/delete/edit”}
{“phase”:“test”,“taskId”:“UI-CORE”,“status”:“done”,“etaSec”:null,“details”:“Add/Toggle/Delete/Edit passed (A11y OK)”}

위 포맷을 벗어나지 말 것.

⸻

8) 테스트/DoD 게이트 (요약)

아래는 Test.md의 요지를 게이트로 재기술한 것이다. 완전히 통과해야 최종 완료.
	•	Unit(TodoService, CoreData Repository): CRUD/정렬/검증/재실행 보존
	•	UI(A11y 기반):
	•	field.input 에서 입력 후 btn.add → list.todo 상단에 추가
	•	row.checkbox 토글 → 취소선/상태 반영, 통계 즉시 갱신
	•	스와이프 삭제(btn.delete) 정상, 연속 동작 crash 0
	•	행 탭 편집(btn.edit) 저장/ESC 취소 플로우 정상
	•	수동:
	•	첫 실행 시 입력창이 최상단 보임
	•	연속 10건 입력 시 포커스 유지, 다크/라이트, SE~Pro Max 레이아웃 정상
	•	실패 기준: 빌드 에러>0 또는 크래시>0 이면 즉시 FAIL
	•	최종 요약 JSON 1줄 출력(통과/실패/경과시간)

⸻

9) 구현 순서(핵심 체크리스트)

Tasks.md 를 소스오브트루스로 따른다(0→7). 최소 체크:
	•	0: Xcode 프로젝트/폴더/스킴/엔트리 구성
	•	1: Domain 엔티티/Repo 프로토콜, Core Data 모델/컨테이너
	•	2: CoreDataTodoRepository, TodoService(add/toggle/delete/edit/stats)
	•	3: TodoViewModel(state: input, todos, stats / intents: onAdd/onToggle/onDelete/onEdit/onChangeInput)
	•	4: ContentView(상단 입력/중간 통계/하단 리스트), TodoRowView(체크박스/스와이프/인라인 편집)
	•	5: 의존 방향/파일 경로/A11y 재검증
	•	6: Unit/UI/수동 테스트 수행 및 통과
	•	7: 요약 JSON 1줄 출력

각 단계 진입/완료 시 JSONL 로그를 남긴다.

⸻

10) 오류 처리와 BLOCKER 규칙
	•	불명확/충돌 명세를 만나면 임의추측 금지. 다음 한 줄을 즉시 기록하고 중단:
{“phase”:“build”,“taskId”:“BLOCKER”,“status”:“error”,“etaSec”:null,“details”:”<문제 요약 및 제안 1안>”}
	•	제안 1안에는 가장 안전한 보수적 해석을 넣는다(예: 입력 검증 강화, UI 위치 유지 등).
	•	BLOCKER 해소 후 같은 단계부터 재개한다.

⸻

11) 품질/스타일(필수 요지)
	•	Swift Concurrency는 본 앱 범위에서 과도 사용 지양(필요 최소한)
	•	확장/재사용을 위해 Repository/UseCase는 프로토콜 우선
	•	View는 상태 최소화와 단방향 데이터 흐름 유지
	•	주석은 “왜(의도/제약)” 중심으로, “무엇/어떻게”는 코드로 드러나게 작성

⸻

12) AI 친화적 문서 구조 (신규 추가)
	•	USER_CONTEXT.md: 사용자 맥락과 의도 파악 (대상 사용자, 핵심 가치, 특별 요구사항)
	•	BUSINESS_RULES.md: 사용자별 비즈니스 로직 커스터마이징 (분류 체계, 시간 관리, 알림 정책)
	•	DECISION_MATRIX.md: AI 의사결정 기준과 우선순위 (가중치, 트레이드오프 규칙)
	•	ADAPTATION_RULES.md: 사용자 입력 기반 동적 적응 규칙 (IF-THEN 로직)
	•	QUALITY_GATES.md: 정량적 품질 기준 (성능, 사용성, 안정성, 보안)
	•	VALIDATION_CHECKLIST.md: 사용자 시나리오 기반 검증 (핵심/스트레스/엣지 케이스)
	•	LEARNING_SIGNALS.md: AI 학습을 위한 성공/실패 지표 (사용자 채택, 성능, 피드백)

⸻

13) 최종 산출물
	•	동작하는 Xcode 프로젝트(OnePageTodo), 위 구조/규칙 완전 준수
	•	테스트 통과 결과와 함께 마지막 요약 JSON 1줄을 stdout 으로 출력

⸻

13) 호출 문구(사용자 1줄 명령)
	•	사용자는 다음 한 문장만 입력해도 된다:
이 문서를 보고 iOS앱을 개발해줘!

이 문장으로 호출되면, 본 CLAUDE.md의 부트 프로토콜을 시작하고, 읽기 순서에 따라 features/*.md, Tasks.md, Test.md 를 자동 탐색/실행하여 완성한다.