# Task.md를 대시보드용 JSON으로 변환하는 프롬프트

## 역할
당신은 Task.md 문서를 분석하여 개발 진행 상황을 모니터링할 수 있는 대시보드용 JSON 파일로 변환하는 전문가입니다.

## 작업 목표
첨부된 Task.md 문서의 "개발 순서" 섹션을 분석하여, 프로젝트의 개발 단계(milestones)와 작업(tasks)을 추출하고 아래 JSON 구조로 변환해주세요.

### Task 정의
- Task.md 문서에서 **들여쓰기 depth로 구분**할 수 있으며 Task는 **Depth 2**에 해당한다
  - **Depth 0** (##): Milestone - 큰 단위 그룹 (예: "1단계: 데이터 모델링")
  - **Depth 1** (###): Task List의 제목 - 중간 그룹 (예: "작업 항목")
  - **Depth 2** (-): **Task** - 실제 작업 단위 (예: "- Habit Entity 정의")
  - **Depth 3** (  -): Task의 세부 내용 (예: "  - id: UUID")
    - "수정 파일", "테스트 체크리스트"는 제외

## JSON 구조

```json
{
  "project": {
    "id": 1,
    "name": "프로젝트 이름",
    "milestones": [
      {
        "id": 1,
        "title": "마일스톤 제목",
        "type": "마일스톤 타입",
        "tasks": [
          {
            "id": 1,
            "title": "작업 제목",
            "title_ux": "작업 제목 UX writing 버전",
            "summary_ux": "작업 요약 UX writing 버전",
            "status": "planned",
            "testStatus": "pending",
            "detail": {
              "description_ux": [
                "첫 번째 설명 - UX writing 버전",
                "두 번째 설명 - UX writing 버전"
              ],
              "modifiedFiles": [
                "FirstFile.swift",
                "SecondFile.swift"
              ],
              "checklist": [
                "첫 번째 체크리스트 항목을 확인하세요",
                "두 번째 체크리스트 항목을 확인하세요"
              ]
            }
          }
        ],
        "taskCount": 0,
        "completedCount": 0
      }
    ],
    "totalTasks": 0,
    "completedTasks": 0,
    "createdAt": "2025-11-06T00:00:00Z"
  }
}
```

## 변환 규칙

### 1. 프로젝트 정보 (project)

| 항목 | 규칙 |
|------|------|
| **id** | 항상 1로 설정 |
| **name** | Task.md의 "프로젝트명" 또는 "앱 이름"에서 추출 - 영문 사용 |
| **createdAt** | 현재 날짜를 ISO 8601 형식으로 설정 (예: "2025-11-06T00:00:00Z") |
| **totalTasks** | 모든 milestone의 task 개수 합계 (계산값) |
| **completedTasks** | 항상 0으로 설정 (초기 상태) |

---

### 2. 마일스톤 (milestones)

Task.md의 "개발 순서" 섹션에 있는 각 단계를 milestone으로 변환합니다.

**마일스톤 구성 (총 3개):**
1. **1단계: 프로젝트 세팅 + 데이터 모델링**
2. **2단계: 기능 구현**
3. **3단계: UI 구현 및 기능 연결**

#### 2-1. id
- 1부터 순차적으로 증가 (1, 2, 3)

#### 2-2. title
- Task.md의 단계 이름 그대로 사용 (개발 용어)
- 예: "프로젝트 세팅 + 데이터 모델링", "기능 구현", "UI 구현 및 기능 연결"

#### 2-3. type
각 단계 이름에 매칭되는 마일스톤 타입 코드:

| title | type |
|-------|------|
| "프로젝트 세팅 + 데이터 모델링" | `"setupAndModeling"` |
| "기능 구현" | `"featureImplementation"` |
| "UI 구현 및 기능 연결" | `"uiImplementation"` |

> **참고**: `type`은 Swift에서 한글 표시명으로 매핑됩니다.
> - `setupAndModeling` → "설계"
> - `featureImplementation` → "기능"
> - `uiImplementation` → "화면"

#### 2-4. taskCount / completedCount
- **taskCount**: 해당 milestone의 task 개수 (계산값)
- **completedCount**: 항상 0으로 설정 (초기 상태)

---

### 3. 작업 (tasks)

각 milestone 내의 세부 작업 항목을 task로 변환합니다.
**"작업 항목 (Task Breakdown)"에 명시된 것만 task로 변환합니다.**

#### 3-1. id
- milestone 내에서 1부터 순차적으로 증가

#### 3-2. title
- Task.md의 작업 항목 이름을 그대로 사용 (개발 용어)
- 예: "Habit Entity 정의", "프로젝트 구조 생성", "MainTabView 개발"

#### 3-3. title_ux (UX Writing)
- **title을 사용자 친화적 표현으로 변환**
- 전문 용어를 일반인이 이해 가능한 단어로 변환
- 의미는 정확하게 유지하되 자연스러운 문장 사용
- 이모지 사용 안 함, 존댓말 유지

**예시:**
| title (개발용) | title_ux (사용자용) |
|----------------|---------------------|
| "Habit Entity 정의" | "저장할 습관 관련 정보 정하기" |
| "MainTabView 개발" | "메인 화면 탭 만들기" |
| "HabitViewModel 구현" | "습관 관리 기능 만들기" |
| "CoreDataManager 구현" | "기록 저장 기능 만들기" |

#### 3-4. summary_ux (UX Writing)
- **글자 수 제한**: 영문 기준 66-74자, 한글 기준 33-37자, 혼용 시 전각 폭 기준 66-74
- **요약 작성**: 작업의 핵심 세부 항목을 글자 수 제한에 맞추어 사용자가 이해하기 쉽도록 설명
- **UX Writing 적용**: 사용자 친화적 표현 사용

**예시:**
| ❌ 나쁜 예시 | ✅ 좋은 예시 |
|-------------|-------------|
| "Core Data Habit Entity 속성 정의 (id, name, emoji, frequency, createdAt, updatedAt)" | "습관의 이름, 아이콘, 반복 주기 등 정보 정하기" |
| "HabitService 로직 구현 - 습관 추가, 수정, 삭제, 조회, 중복 검증 로직" | "습관 추가, 수정, 삭제 등 핵심 기능 구현하기" |
| "MainTabView 개발 - TabView 구조 구현, 습관 목록 탭" | "화면 전환 구조와 주요 탭 만들기" |

#### 3-5. status
- **초기 상태**: 항상 `"planned"`로 설정
- **예외**: "Xcode 프로젝트 생성" 작업만 `"completed"`로 설정

**가능한 값:**
| 코드 값 | Swift 매핑 |
|---------|-----------|
| `"planned"` | "예정" |
| `"inProgress"` | "진행 중" |
| `"completed"` | "완료" |
| `"onHold"` | "보류" |

#### 3-6. testStatus
테스트 가능 여부와 진행 상태를 나타냅니다.

**설정 규칙:**
- **UI 구현 단계 (milestone.type === "uiImplementation")** + **"View 개발" 또는 "화면 구현" 포함**
  → 초기 상태: `"pending"`
  → 이후 해당 태스크 작업 완료시 `"ready"`로 변경
  → 이후 사용자 테스트 중 수정을 요청받았을 때 `"checking"`로 변경
  → 사용자 테스트 완료시 `"checked"`로 변경
- **ViewModel, Component, Helper 등** → `"excluded"`
- **나머지** → `"excluded"`

**가능한 값:**
| 코드 값 | Swift 매핑 | 의미 |
|---------|-----------|------|
| `"ready"` | "테스트 가능" | 테스트 가능한 상태 |
| `"pending"` | "테스트 대기" | 아직 테스트 안 함 |
| `"checking"` | "테스트 진행 중" | 테스트 진행 중 |
| `"checked"` | "테스트 완료" | 사용자가 테스트 완료 |
| `"excluded"` | (UI에서 숨김) | 테스트 불필요 |

---

### 4. 상세 정보 (detail)

태스크 카드를 클릭했을 때 상세 화면에 표시될 정보입니다.

#### 4-1. description_ux (UX Writing 적용)
- **타입**: 문자열 배열 `string[]`
- **내용**: 해당 작업(Task)의 세부 내용을 사용자 친화적으로 자세하게 설명
- **형식**: 3-5개의 문장으로 구성된 배열
- **UX Writing**: 전문 용어를 피하고 자연스러운 설명 사용

**예시:**
```json
{
  "description_ux": [
    "화면 전환 구조를 만들어요",
    "습관 목록, 통계, 캘린더 화면을 구성해요",
    "하단 탭 바를 통해 쉽게 화면을 전환할 수 있어요"
  ]
}
```

#### 4-2. modifiedFiles
- **타입**: 문자열 배열 `string[]`
- **내용**: 해당 작업에서 수정되거나 생성되는 파일 목록

**추출 규칙:**
- Task.md의 "**수정 파일**" 섹션에서 추출
- 리스트 항목을 배열로 변환
- 백틱(`)으로 감싸진 파일명에서 백틱 제거
- 파일 경로가 포함된 경우 전체 경로 유지
- 섹션이 없으면 빈 배열 `[]` 반환

**예시:**
```json
{
  "modifiedFiles": [
    "MainTabView.swift",
    "HabitListView.swift",
    "StatisticsView.swift"
  ]
}
```

#### 4-3. checklist (조건부)
- **타입**: 문자열 배열 `string[]`
- **내용**: 사용자가 테스트할 때 확인해야 할 체크리스트

**조건부 포함:**
- **testStatus가 `"excluded"`인 경우** → checklist 필드 자체를 생략
- **UI 단계의 테스트 대상 작업** → checklist 포함

**추출 규칙:**
- Task.md의 "**테스트 체크리스트**" 섹션에서 추출

**예시:**
```json
{
  "checklist": [
    "시뮬레이터 빌드가 잘 되나요?",
    "탭을 눌렀을 때 해당 화면으로 전환되나요?",
    "각 탭에 올바른 화면이 표시되나요?"
  ]
}
```

---

## 특수 규칙

### 1. Xcode 프로젝트 생성 작업
```json
{
  "title": "Xcode 프로젝트 생성",
  "title_ux": "Xcode 프로젝트 준비하기",
  "summary_ux": ".xcodeproj 파일 생성",
  "status": "completed",  // ← 유일하게 초기 상태가 completed
  "testStatus": "excluded",
}
```

### 2. 숫자 필드 정확한 계산
- `milestone.taskCount`: 해당 마일스톤의 task 개수
- `milestone.completedCount`: 항상 0 (초기 상태)
- `project.totalTasks`: 모든 task 개수 합계
- `project.completedTasks`: 항상 0 (초기 상태)

### 3. 날짜 형식
- ISO 8601 형식 사용: `"2025-11-06T00:00:00Z"`

---

## 출력 형식
- 반드시 유효한 JSON 형식으로만 출력
- 주석이나 설명 없이 순수 JSON만 반환
- JSON 문법 오류가 없어야 함 (마지막 쉼표 주의)
- 들여쓰기는 2칸 스페이스 사용

---

## 예시

```markdown
# Daily Habit Tracker - Task.md

## 프로젝트 정보
- 앱 이름: Daily Habit Tracker

## 개발 순서

### 1단계: 프로젝트 세팅 + 데이터 모델링

#### 작업 항목 (Task Breakdown)

##### 1. Xcode 프로젝트 생성

##### 2. Habit Entity 정의
- **상세 설명**:
  - 습관의 기본 정보를 저장할 데이터 구조를 만들어요
  - 이름, 아이콘, 반복 주기 등의 속성을 정해요
- **수정 파일**:
  - `Habit.swift`
  - `PersistenceController.swift`

### 3단계: UI 구현 및 기능 연결

#### 작업 항목 (Task Breakdown)

##### 1. MainTabView 개발
- **상세 설명**:
  - 화면 전환 구조를 만들어요
  - 습관 목록, 통계, 캘린더 화면을 구성해요
  - 하단 탭 바를 통해 쉽게 화면을 전환할 수 있어요
- **수정 파일**:
  - `MainTabView.swift`
  - `HabitListView.swift`
  - `StatisticsView.swift`
- **테스트 체크리스트**:
  - 시뮬레이터 빌드가 잘 되나요?
  - 탭을 눌렀을 때 해당 화면으로 전환되나요?
  - 각 탭에 올바른 화면이 표시되나요?
```

### 출력 (JSON)

```json
{
  "project": {
    "id": 1,
    "name": "Daily Habit Tracker",
    "milestones": [
      {
        "id": 1,
        "title": "프로젝트 세팅 + 데이터 모델링",
        "type": "setupAndModeling",
        "tasks": [
          {
            "id": 1,
            "title": "Xcode 프로젝트 생성",
            "title_ux": "Xcode 프로젝트 준비하기",
            "summary_ux": ".xcodeproj 파일 생성",
            "status": "completed",
            "testStatus": "excluded",
            "detail": {
              "description_ux": [
                "Xcode에서 새 프로젝트를 만들어요"
              ],
              "modifiedFiles": []
            }
          },
          {
            "id": 2,
            "title": "Habit Entity 정의",
            "title_ux": "저장할 습관 관련 정보 정하기",
            "summary_ux": "습관의 이름, 아이콘, 반복 주기 등 정보 정하기",
            "status": "planned",
            "testStatus": "excluded",
            "detail": {
              "description_ux": [
                "습관의 기본 정보를 저장할 데이터 구조를 만들어요",
                "이름, 아이콘, 반복 주기 등의 속성을 정해요"
              ],
              "modifiedFiles": [
                "Habit.swift",
                "PersistenceController.swift"
              ]
            }
          }
        ],
        "taskCount": 2,
        "completedCount": 0
      },
      {
        "id": 3,
        "title": "UI 구현 및 기능 연결",
        "type": "uiImplementation",
        "tasks": [
          {
            "id": 1,
            "title": "MainTabView 개발",
            "title_ux": "메인 화면 탭 만들기",
            "summary_ux": "화면 전환 구조와 주요 탭 만들기",
            "status": "planned",
            "testStatus": "pending",
            "detail": {
              "description_ux": [
                "화면 전환 구조를 만들어요",
                "습관 목록, 통계, 캘린더 화면을 구성해요",
                "하단 탭 바를 통해 쉽게 화면을 전환할 수 있어요"
              ],
              "modifiedFiles": [
                "MainTabView.swift",
                "HabitListView.swift",
                "StatisticsView.swift"
              ],
              "checklist": [
                "시뮬레이터 빌드가 잘 되나요?",
                "탭을 눌렀을 때 해당 화면으로 전환되나요?",
                "각 탭에 올바른 화면이 표시되나요?"
              ]
            }
          }
        ],
        "taskCount": 1,
        "completedCount": 0
      }
    ],
    "totalTasks": 3,
    "completedTasks": 0,
    "createdAt": "2025-11-06T00:00:00Z"
  }
}
```

---

이제 첨부된 Task.md 문서를 위 규칙에 따라 JSON으로 변환해주세요.
