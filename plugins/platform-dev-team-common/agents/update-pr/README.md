# update-pr Agent

PR 제목과 설명을 한 번에 업데이트하는 통합 워크플로우 에이전트입니다.

> **Agent Type**: 이것은 **오케스트레이터 에이전트**입니다. 단일 작업을 수행하는 스킬과 달리, 에이전트는 여러 커맨드(update-pr-title, update-pr-desc)를 조율하여 완전한 PR 업데이트 워크플로우를 실행합니다.

## 개요

**활성화**: 사용자가 PR 업데이트를 요청할 때

**목적**: PR 제목과 설명을 한 번에 업데이트하는 편리한 통합 워크플로우 제공

## 기능

- **통합 워크플로우**: update-pr-title과 update-pr-desc 명령어를 자동으로 실행
- **선택적 업데이트**: 제목만, 설명만, 또는 둘 다 업데이트 가능
- **프로젝트 인식**: 프로젝트 타입(Django, FastAPI, React, Vue 등)에 따라 콘텐츠 자동 적응
- **언어 지원**: 한글/영어 선택 가능
- **일관성 보장**: 제목과 설명이 서로 일치하도록 보장

## 사용 방법

### 기본 사용 (제목 + 설명 모두 업데이트)

```bash
/update-pr
/update-pr --pr 123
/update-pr --lang eng
/update-pr --pr 123 --lang ko --include-load-test
```

### 제목만 업데이트

사용자가 명시적으로 "제목만" 또는 "title only" 요청 시:

```
update pr title for PR 123
```

### 설명만 업데이트

사용자가 명시적으로 "설명만" 또는 "description only" 요청 시:

```
update pr description with load test results
```

## 옵션

- `--pr <number>`: PR 번호 (생략 시 현재 브랜치에서 자동 감지)
- `--lang <language>`: 언어 설정
  - `korean` 또는 `ko`: 한국어 (기본값)
  - `english` 또는 `eng`: 영어
- `--include-load-test`: 로드 테스트 결과 포함 (설명 업데이트 시)

## 워크플로우

1. **옵션 파싱**: 명령어 인자에서 PR 번호, 언어, 옵션 추출
2. **PR 감지**: PR 번호가 없으면 현재 브랜치에서 PR 자동 감지
3. **제목 업데이트**: update-pr-title 명령어 실행
   - 커밋 메시지 분석
   - 변경 사항 요약
   - 티켓 ID 보존
   - 72자 이하 제목 생성
4. **설명 업데이트**: update-pr-desc 명령어 실행
   - 프로젝트 타입 자동 감지
   - 변경 사항 상세 분석
   - Mermaid 차트 자동 생성
   - PR 템플릿 준수
5. **결과 보고**: 업데이트된 내용 요약 및 PR URL 제공

## 프로젝트 타입 자동 감지

update-pr 스킬은 프로젝트 타입을 자동으로 감지하여 적절한 콘텐츠를 생성합니다:

### 백엔드 프로젝트
- **Django**: ViewSet, Serializer, Model 용어 사용
- **FastAPI**: Route Handler, Pydantic Schema 용어 사용
- **Express**: Controller, Middleware 용어 사용
- **NestJS**: Controller, Service, Repository 용어 사용

### 프론트엔드 프로젝트
- **React**: Component, Hook, State 용어 사용
- **Vue**: Component, Composable, Reactive Data 용어 사용
- **Angular**: Component, Service, Observable 용어 사용

### 인프라 프로젝트
- **Terraform**: Resource, Module 용어 사용
- **Kubernetes**: Pod, Service, Deployment 용어 사용

## 다른 스킬/명령어와의 통합

- **update-pr-title 명령어**: PR 제목 생성
  - 커밋 메시지 규칙 준수
  - 티켓 ID 자동 추출
  - 한글/영어 지원

- **update-pr-desc 명령어**: PR 설명 생성
  - 프로젝트 타입별 콘텐츠 적응
  - 필수 Mermaid 차트 포함
  - PR 템플릿 준수

- **mermaid-expert 스킬**: Mermaid 차트 생성 (update-pr-desc 내부에서 사용)
  - 라이트/다크 모드 호환
  - 프로젝트 타입별 다이어그램

## 예시

### 예시 1: 현재 브랜치 PR 전체 업데이트 (한글)

```bash
User: /update-pr

Skill:
현재 브랜치에서 PR 감지: #156

제목 업데이트 중...
✓ 제목: [SYN-1234] 기능: JWT 인증 시스템 구현

설명 업데이트 중...
✓ 프로젝트 타입: Django Backend
✓ PR 타입: Feature ✨
✓ Mermaid 차트: API Flow Diagram
✓ 테스트 커버리지: 87%

PR #156 업데이트 완료!
보기: https://github.com/org/repo/pull/156
```

### 예시 2: 특정 PR, 영어로 업데이트

```bash
User: /update-pr --pr 123 --lang eng

Skill:
Updating PR #123...

Title updated:
✓ Title: [SYN-1234] feat: Implement JWT authentication system

Description updated:
✓ Project type: FastAPI Backend
✓ PR Type: Feature ✨
✓ Mermaid chart: API Flow Diagram
✓ Test coverage: 92%

PR #123 updated successfully!
View: https://github.com/org/repo/pull/123
```

### 예시 3: 설명만 업데이트 (로드 테스트 포함)

```bash
User: update pr description for #156 with load test results

Skill:
설명만 업데이트 중...

✓ 프로젝트 타입: React Frontend
✓ PR 타입: Performance ⚡
✓ Mermaid 차트: Component Hierarchy
✓ 로드 테스트 결과 포함
✓ Lighthouse 점수 개선: 75 → 92

PR #156 설명 업데이트 완료!
```

### 예시 4: 제목만 업데이트

```bash
User: update pr title for current branch

Skill:
현재 브랜치 PR: #178

제목 업데이트 중...
✓ 제목: [SYN-5678] 수정: 사용자 프로필 이미지 업로드 버그 해결

PR #178 제목 업데이트 완료!
```

## 장점

1. **편리함**: 하나의 명령어로 PR 전체 업데이트
2. **일관성**: 제목과 설명이 서로 일치하고 연관성 있음
3. **유연성**: 필요에 따라 제목만, 설명만, 또는 둘 다 업데이트
4. **프로젝트 인식**: 자동으로 프로젝트 타입 감지 및 적응
5. **사용자 친화적**: 복잡한 작업을 간단한 인터페이스로 제공
6. **시간 절약**: 수동 작업 대비 시간 대폭 절약
7. **품질 보장**: PR 템플릿 준수 및 필수 섹션 자동 포함

## 오류 처리

- **PR 번호 미제공 및 브랜치에서 감지 불가**: 사용자에게 PR 번호 요청
- **GitHub MCP 서버 연결 실패**: GITHUB_TOKEN 및 .mcp.json 확인 안내
- **명령어 실행 실패**: 구체적인 오류 메시지 및 해결 방법 제공
- **사용자 의도 불명확**: 명확화를 위한 질문

## 제한 사항

- GitHub MCP 서버가 설정되어 있어야 함
- GITHUB_TOKEN이 PR 읽기/쓰기 권한을 가져야 함
- Git 저장소 내에서 실행되어야 함

## 참고 자료

- [update-pr-title 명령어](../../commands/update-pr-title.md)
- [update-pr-desc 명령어](../../commands/update-pr-desc.md)
- [mermaid-expert 스킬](../mermaid-expert/README.md)
- [commit-with-message 스킬](../commit-with-message/README.md)
