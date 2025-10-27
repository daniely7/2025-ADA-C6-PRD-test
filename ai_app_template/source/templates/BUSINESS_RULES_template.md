# BUSINESS_RULES.md — 비즈니스 규칙 & 커스터마이징

> 사용자별 요구사항에 따라 달라져야 하는 비즈니스 로직과 규칙들

## 📋 작성 가이드
USER_CONTEXT.md의 정보를 바탕으로, 구체적인 비즈니스 로직을 정의합니다.
AI는 이 규칙들을 참고하여 코드 구현 시 세부사항을 결정합니다.

---

## 📂 데이터 분류 체계

### 분류 방식 선택
- [ ] **없음**: 단순한 리스트만
- [ ] **카테고리**: {{CATEGORY_OPTIONS}}
- [ ] **프로젝트**: 특정 목표 중심
- [ ] **우선순위**: High/Medium/Low  
- [ ] **날짜**: 오늘/이번주/다음주
- [ ] **상태**: {{STATUS_OPTIONS}}
- [ ] **커스텀**: [사용자 정의 분류]

### 분류별 세부 규칙
**선택한 분류 방식에 대한 구체적 규칙:**

```markdown
예시 - {{CLASSIFICATION_EXAMPLE_TYPE}}:
- {{CLASSIFICATION_RULE_1}}
- {{CLASSIFICATION_RULE_2}}
- {{CLASSIFICATION_RULE_3}}
- {{CLASSIFICATION_RULE_4}}
```

---

## ⏰ 시간 관리 규칙

### {{TIME_FIELD_NAME}} 정책
- **{{TIME_FIELD_NAME}} 설정**: [필수 / 선택적 / 없음]
- **{{TIME_FIELD_NAME}} 알림**: {{NOTIFICATION_OPTIONS}}
- **지난 {{TIME_FIELD_NAME}}**: {{OVERDUE_HANDLING}}

### 반복 작업 규칙
- **반복 패턴**: {{RECURRING_PATTERNS}}
- **반복 생성 시점**: {{RECURRING_TIMING}}
- **반복 예외**: {{RECURRING_EXCEPTIONS}}

**예시:**
```markdown
{{TIME_MANAGEMENT_EXAMPLE}}
```

---

## 🔔 알림 & 피드백 정책

### 알림 시나리오
- **새 항목 추가 시**: [무음 / 햅틱 / 소리]
- **항목 완료 시**: {{COMPLETION_FEEDBACK}}
- **{{TIME_FIELD_NAME}} 접근 시**: {{TIME_ALERT_OPTIONS}}
- **목표 달성 시**: {{ACHIEVEMENT_FEEDBACK}}

### 피드백 & 보상
- **진행률 표시**: {{PROGRESS_DISPLAY_OPTIONS}}
- **성취 배지**: {{ACHIEVEMENT_SYSTEM}}
- **동기부여 메시지**: {{MOTIVATION_MESSAGES}}

**예시:**
```markdown
{{NOTIFICATION_EXAMPLE}}
```

---

## 📊 데이터 & 동기화 정책

### 데이터 보존 규칙
- **완료된 항목**: {{COMPLETED_ITEM_HANDLING}}
- **삭제된 항목**: {{DELETED_ITEM_HANDLING}}
- **백업 주기**: {{BACKUP_FREQUENCY}}

### 동기화 & 공유
- **클라우드 동기화**: {{CLOUD_SYNC_OPTIONS}}
- **기기 간 동기화**: {{DEVICE_SYNC_OPTIONS}}
- **공유 기능**: {{SHARING_OPTIONS}}

### 프라이버시 정책
- **데이터 암호화**: {{ENCRYPTION_REQUIREMENTS}}
- **외부 서비스 연동**: {{EXTERNAL_SERVICE_POLICY}}
- **사용 분석**: {{ANALYTICS_POLICY}}

**예시:**
```markdown
{{DATA_POLICY_EXAMPLE}}
```

---

## 🎯 사용자 행동 기반 자동화

### 학습 기반 제안
- **자주 쓰는 키워드**: {{KEYWORD_LEARNING}}
- **시간 패턴**: {{TIME_PATTERN_LEARNING}}
- **완료 패턴**: {{COMPLETION_PATTERN_LEARNING}}

### 컨텍스트 인식
- **위치 기반**: {{LOCATION_BASED_FEATURES}}
- **시간 기반**: {{TIME_BASED_FEATURES}}
- **{{EXTERNAL_CONTEXT}} 연동**: {{EXTERNAL_CONTEXT_FEATURES}}

**예시:**
```markdown
{{AUTOMATION_EXAMPLE}}
```

---

## 🔧 UI/UX 개인화 규칙

### 표시 옵션
- **기본 보기**: {{DEFAULT_VIEW_OPTIONS}}
- **정렬 순서**: {{SORT_OPTIONS}}
- **완료된 항목**: {{COMPLETED_DISPLAY_OPTIONS}}

### 접근성 & 사용성
- **폰트 크기**: {{FONT_SIZE_OPTIONS}}
- **색상 테마**: {{COLOR_THEME_OPTIONS}}
- **햅틱 피드백**: {{HAPTIC_OPTIONS}}

**예시:**
```markdown
{{UI_PERSONALIZATION_EXAMPLE}}
```

---

## 📝 앱별 특화 규칙

### {{APP_SPECIFIC_SECTION_1}}
{{APP_SPECIFIC_CONTENT_1}}

### {{APP_SPECIFIC_SECTION_2}}
{{APP_SPECIFIC_CONTENT_2}}

### {{APP_SPECIFIC_SECTION_3}}
{{APP_SPECIFIC_CONTENT_3}}

**예시:**
```markdown
{{APP_SPECIFIC_EXAMPLE}}
```