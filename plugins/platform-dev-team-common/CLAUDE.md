# Platform Dev Team - Claude 작업 가이드

## 커밋 메시지 규칙

본 저장소의 커밋 메시지는 다음 규칙을 따릅니다:

### 언어 선택

커밋 메시지는 **한글** 또는 **영어**로 작성할 수 있습니다.

- **기본 언어**: 한글 (특별한 요청이 없으면 한글 사용)
- **영어 사용**: 오픈소스 기여, 국제 협업 시 또는 사용자가 명시적으로 요청하는 경우

### 기본 원칙 (한글)

1. **한글로 작성한다**
   - 모든 커밋 메시지는 한글로 작성합니다
   - 기술 용어는 필요 시 영문 그대로 사용 가능합니다 (예: TDD, API, ViewSet)

2. **변경사항에 대해 간결하고 명확하게 작성한다**
   - 무엇을 변경했는지 한 줄로 요약합니다
   - 불필요한 설명은 생략합니다
   - 명령형으로 작성합니다 (예: "추가한다" → "추가")

### 기본 원칙 (영어)

1. **Use imperative mood**
   - Write commit messages in imperative mood
   - Good: "Add", "Fix", "Update"
   - Bad: "Added", "Adds", "Adding"

2. **Be concise and clear**
   - Summarize what changed in one line
   - Omit unnecessary details
   - Use command form

### 커밋 메시지 형식

**한글**:
```
<타입>: <간결한 설명>

[선택] 상세 설명 (필요한 경우)
```

**영어**:
```
<type>: <brief description>

[optional] Detailed explanation
```

### 타입 분류

**한글**:
- **기능**: 새로운 기능 추가
- **수정**: 버그 수정
- **문서**: 문서 변경
- **스타일**: 코드 포맷팅, 세미콜론 누락 등 (동작 변경 없음)
- **리팩토링**: 코드 리팩토링 (기능 변경 없음)
- **테스트**: 테스트 코드 추가 또는 수정
- **빌드**: 빌드 시스템 또는 외부 종속성 변경

**영어**:
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code formatting (no behavior change)
- **refactor**: Code refactoring (no feature change)
- **test**: Test code addition or modification
- **build**: Build system or dependency changes

### 예시

**좋은 커밋 메시지 (한글)**:
```
기능: TDD 스킬 추가

Kent Beck의 TDD 방법론과 Tidy First 원칙을 따르는 스킬 구현
Red → Green → Refactor 사이클 가이드 포함
```

```
수정: PR 제목 생성 시 티켓 ID 보존 오류 수정
```

```
문서: CalVer 버전 관리 가이드 추가
```

**좋은 커밋 메시지 (영어)**:
```
feat: Add TDD skill

Implement skill following Kent Beck's TDD and Tidy First principles
Include Red → Green → Refactor cycle guide
```

```
fix: Preserve ticket ID when generating PR title
```

```
docs: Add CalVer versioning guide
```

**나쁜 커밋 메시지**:
```
Updated files  # 한글이 기본이므로 특별한 이유 없이 영어 사용, 구체적이지 않음
```

```
여러 가지 수정함  # 무엇을 수정했는지 불명확
```

```
TDD skill을 추가하고, docs-manager도 같이 만들고, README도 업데이트하고...  # 너무 장황함, 커밋 분리 필요
```

### 추가 규칙

1. **제목은 72자 이하**로 작성합니다
2. **하나의 커밋은 하나의 논리적 변경**만 포함합니다
3. **티켓 ID가 있는 경우** 제목 앞에 포함합니다 (예: `[SYN-1234] 기능: 사용자 인증 추가`)
4. **Breaking Changes가 있는 경우** 커밋 메시지 본문에 명시합니다

### Co-Authored-By

Claude와 협업한 커밋에는 다음을 추가합니다:

```
Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```
