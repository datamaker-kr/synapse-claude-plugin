# Changelog

All notable changes to the Platform Dev Team Claude Plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Calendar Versioning (CalVer)](https://calver.org/) using YYYY.Minor.Patch format.

## [Unreleased]

### Changed

#### 문서

- **changelog-manager 문서 정리**: 불필요한 이전 버전 관련 경고 문구 제거
  - v1.3.13 이후 CalVer 사용 경고 문구 제거 (이미 적용됨)
  - 버전 형식 설명을 `YYYY.Minor.Patch`로 통일
  - 예시 버전을 2026년 기준으로 업데이트
  - 영향 파일: `commands/add-changelog.md`, `skills/changelog-manager/README.md`, `skills/changelog-manager/SKILL.md`

---

## [2026.2.1] - 2026-01-19

### Added

#### 스킬

- **changelog-manager Skill**: CHANGELOG.md 관리 및 변경 이력 자동 생성
  - Keep a Changelog 형식 준수
  - CalVer (YYYY.MM.MICRO) 버전 관리
  - Git 커밋 및 변경사항 자동 분석
  - 티켓 ID 자동 추출 (브랜치 이름에서)
  - 한글/영어 다국어 지원
  - 카테고리별 자동 분류 (Added, Changed, Fixed 등)
  - `/add-changelog` 커맨드로 호출
  - [상세 가이드](skills/changelog-manager/README.md)

#### 커맨드

- **add-changelog Command**: CHANGELOG.md에 변경 이력 추가
  - Unreleased 섹션에 자동 추가
  - Keep a Changelog 형식 자동 준수
  - 티켓 ID 기반 JIRA 링크 자동 생성
  - `--type`: 카테고리 지정 (added/changed/fixed)
  - `--lang`: 언어 지정 (korean/english)
  - `--ticket`: 티켓 ID 수동 지정
  - [상세 가이드](commands/add-changelog.md)

---

## [2026.2.0] - 2026-01-19

### Added

#### 에이전트

- **planner Agent**: 포괄적이고 실행 가능한 구현 계획 수립 전문가 (오케스트레이터)
  - 4단계 계획 프로세스: 요구사항 분석 → 아키텍처 검토 → 단계별 분해 → 구현 순서
  - 각 단계마다 명확한 액션, 파일 경로, 의존성, 복잡도, 위험 포함
  - 기능 구현, 아키텍처 변경, 복잡한 리팩토링 요청 시 활성화
  - 출력: 상세한 구현 계획 (테스트 전략, 롤백 계획, 성공 기준 포함)
  - Opus 모델 사용으로 고품질 계획 생성
  - [상세 가이드](agents/planner/README.md)

- **docs-manager Agent**: 포괄적인 문서 관리 오케스트레이터
  - 모놀리식 스킬을 오케스트레이터 패턴으로 리팩토링
  - docs-analyzer, docs-bootstrapper, mermaid-expert 스킬 조율
  - 6단계 워크플로우: 분석 → 부트스트랩 → 다이어그램 → 권장사항 → 승인 → 업데이트
  - [상세 가이드](agents/docs-manager/README.md)

- **update-pr Agent**: PR 제목과 설명을 한 번에 업데이트하는 통합 워크플로우 오케스트레이터
  - update-pr-title과 update-pr-desc 명령어를 자동으로 실행
  - 선택적 업데이트 지원 (제목만, 설명만, 또는 둘 다)
  - 프로젝트 타입에 따라 콘텐츠 자동 적응
  - 한글/영어 지원
  - [상세 가이드](agents/update-pr/README.md)

#### 스킬

- **docs-analyzer Skill**: 코드 변경사항 분석 및 문서 갭 식별
  - Git 히스토리 분석
  - 기존 문서 카탈로그
  - 구조화된 분석 리포트 생성
  - 우선순위별 갭 목록 (Critical, High, Medium, Low)
  - 독립 실행 또는 docs-manager 에이전트에 의해 호출
  - [상세 가이드](skills/docs-analyzer/README.md)

- **docs-bootstrapper Skill**: 프로젝트 문서 구조 부트스트랩
  - 프로젝트 타입 자동 감지 (Django, FastAPI, React, Vue, Terraform, Kubernetes 등)
  - 초기 README.md 생성
  - Architecture 및 API 문서 템플릿 제공
  - 템플릿 기반 생성 (README, architecture, api)
  - 독립 실행 또는 docs-manager 에이전트에 의해 호출
  - [상세 가이드](skills/docs-bootstrapper/README.md)

### Changed

#### 아키텍처

- **에이전트와 스킬 분리**: 오케스트레이터를 별도 `agents/` 디렉토리로 이동
  - **구조 개선**: 에이전트(오케스트레이터)와 스킬(작업자) 간 명확한 구분
  - **이동된 에이전트**:
    - `skills/docs-manager/` → `agents/docs-manager/` (문서 관리 워크플로우)
    - `skills/update-pr/` → `agents/update-pr/` (PR 업데이트 워크플로우)
    - `skills/planner/` → `agents/planner/` (구현 계획 프로세스)
  - **남아있는 스킬** (workers):
    - `skills/docs-analyzer/` (문서 갭 분석)
    - `skills/docs-bootstrapper/` (문서 부트스트랩)
    - `skills/mermaid-expert/` (다이어그램 생성)
    - `skills/commit-message/` (커밋 메시지 규칙)
    - `skills/tdd-workflow/` (TDD 가이드)
  - **plugin.json 업데이트**: `"agents": "./agents/"` 필드 추가
  - **문서 업데이트**: README.md에 아키텍처 섹션 추가, 에이전트 vs 스킬 구분 명확화
  - **에이전트 타입 명시**: 각 에이전트의 SKILL.md와 README.md에 "Agent Type" 섹션 추가
  - **교차 참조 업데이트**: 모든 문서 링크를 새 경로로 업데이트

**디렉토리 구조**:
```
platform-dev-team-claude-plugin/
├── agents/                    # 오케스트레이터 에이전트
│   ├── docs-manager/          # 문서 관리 워크플로우
│   ├── update-pr/             # PR 업데이트 워크플로우
│   └── planner/               # 구현 계획 프로세스
├── skills/                    # 전문 작업자 스킬
│   ├── docs-analyzer/
│   ├── docs-bootstrapper/
│   ├── mermaid-expert/
│   ├── commit-message/
│   └── tdd-workflow/
└── commands/
```

**아키텍처 명확성 개선**:
- **에이전트(Agents)**: 다단계 워크플로우를 관리하고 다른 스킬/커맨드를 조율하는 오케스트레이터
- **스킬(Skills)**: 단일 책임을 가진 전문 작업자로, 독립적으로 사용되거나 에이전트에 의해 호출됨

#### 스킬

- **TDD Skill → TDD Workflow**: 스킬 이름 변경으로 명확성 개선
  - 디렉토리: `skills/tdd-skill/` → `skills/tdd-workflow/`
  - 스킬 이름: `tdd-skill` → `tdd-workflow`
  - 워크플로우 중심의 명명으로 목적 명확화

#### 커맨드

- **update-pr-desc**: 프로젝트 타입별 자동 대응으로 범용성 대폭 개선
  - **프로젝트 타입 자동 감지**: Django, FastAPI, Express, NestJS, React, Vue, Angular, Terraform, Kubernetes 등 지원
  - **백엔드 프로젝트 지원**:
    - Django: ViewSet, Serializer, Model, Migration 용어
    - FastAPI: Route Handler, Pydantic Schema, Alembic 용어
    - Express/NestJS: Controller, Service, Middleware 용어
  - **프론트엔드 프로젝트 지원**:
    - React: Component, Hook, State Management 용어
    - Vue: Component, Composable, Vuex 용어
    - Angular: Component, Service, NgRx 용어
  - **인프라 프로젝트 지원**:
    - Terraform: Resource, Module 용어
    - Kubernetes: Pod, Service, Deployment 용어
  - **프로젝트별 Mermaid 차트**:
    - 백엔드: API Flow, ER Diagram, Process Flowchart
    - 프론트엔드: Component Hierarchy, State Management Flow
    - 인프라: Resource Architecture
  - **프로젝트별 테스트 명령어**: Django (`python manage.py test`), FastAPI (`pytest`), React (`npm test`) 등
  - **프로젝트별 배포 가이드**: 각 프레임워크에 맞는 배포 절차 및 명령어 제공

### Moved

- **mermaid-guidelines.md**: docs-manager → mermaid-expert
  - 더 적절한 위치로 이동
  - mermaid-expert 스킬과 함께 위치
  - 가이드라인 접근성 개선

---

## [2026.1.3] - 2026-01-19

### Fixed

#### 플러그인 스키마

- **plugin.json**: Claude Code 플러그인 스키마 검증 오류 수정
  - `repository` 필드를 객체에서 문자열로 변경
  - `displayName` 필드 제거 (스키마에서 미지원)
  - `icon` 필드 제거 (스키마에서 미지원)

#### 문서

- **README.md**: 설치 방법 업데이트
  - 마켓플레이스 방식을 GitHub 직접 설치로 변경
  - 더 신뢰성 높은 설치 방법 제공

---

## [2026.1.2] - 2026-01-19

### Changed

#### 문서

- **README.md**: "다른 프로젝트에서 사용하기" 섹션 추가
  - 옵션 1: Claude Code 마켓플레이스 (권장)
  - 옵션 2: Git Submodule (팀 프로젝트용)
  - 옵션 3: 심볼릭 링크 (개발자용)
  - 옵션 4: Git Worktree (고급 사용자용)
  - 각 옵션별 장단점, 사용 방법, 권장 사항 포함
  - synapse-backend, synapse-workspace, synapse-annotator 등 여러 프로젝트에서 플러그인 사용 가이드

---

## [2026.1.1] - 2026-01-18 - First Release

**플러그인 초기 릴리스**

이 릴리스는 Platform Dev Team을 위한 Claude 플러그인의 첫 번째 공식 릴리스입니다.

### Added

#### 스킬 (4개)

- **TDD Workflow**: Kent Beck의 TDD 및 Tidy First 방법론 가이드
  - Red → Green → Refactor 사이클
  - 구조적/행동적 변경 분리
  - 테스트 통과 시에만 커밋
  - [상세 가이드](skills/tdd-workflow/README.md)

- **docs-manager Skill**: 적극적인 문서 모니터링 및 Mermaid 차트 생성
  - 코드 변경 자동 감지
  - 문서 업데이트 제안
  - Mermaid 차트 자동 생성
  - [상세 가이드](skills/docs-manager/README.md)

- **mermaid-expert Skill**: 라이트/다크 모드 호환 다이어그램 전문 생성
  - 6가지 다이어그램 유형 템플릿
  - 의미론적 색상 사용
  - 순수 검정/흰색 금지 규칙
  - [상세 가이드](skills/mermaid-expert/README.md)

- **commit-message Skill**: 한글/영어 커밋 메시지 작성 규칙 자동 적용
  - 언어 선택 (한글 기본, 영어 선택 가능)
  - 커밋 타입 자동 선택
  - 72자 제한 적용
  - Co-Authored-By 자동 추가
  - [상세 가이드](skills/commit-message/README.md)

#### 커맨드 (3개)

- **`/update-pr-title`**: PR 제목 자동 생성
  - 커밋 메시지와 변경사항 분석
  - 한글/영어 지원
  - 티켓 ID 보존
  - 72자 이하 제목 생성

- **`/update-pr-desc`**: 포괄적인 PR 설명 생성
  - 필수 Mermaid 차트 포함
  - PR 템플릿 준수
  - API/모델 변경 감지
  - 한글/영어 지원

- **`/update-docs`**: 문서 업데이트 권장사항 제시
  - 저장소 문서 스캔
  - 오래된 문서 식별
  - Mermaid 차트 생성
  - 사용자 승인 후 업데이트

#### 주요 기능

- **GitHub MCP 서버 통합**
  - PR 읽기/쓰기
  - 커밋 이력 분석
  - 파일 변경사항 추적

- **CalVer 버전 관리**
  - YYYY.Minor.Patch 형식
  - 명확한 버전 증가 규칙
  - 자동화된 버전 관리

- **이중 언어 지원**
  - 한국어 (기본)
  - 영어 (선택 가능)
  - 모든 커맨드와 스킬에서 지원

- **Mermaid 차트 시스템**
  - 라이트/다크 모드 호환
  - 엄격한 색상 규칙
  - 6가지 다이어그램 유형
  - 의미론적 색상 사용

#### 문서

- **각 스킬별 상세 README**
  - skills/tdd-workflow/README.md
  - skills/docs-manager/README.md
  - skills/mermaid-expert/README.md
  - skills/commit-message/README.md

- **CLAUDE.md**: Claude 작업 가이드
  - 커밋 메시지 규칙 (한글/영어)
  - 타입 분류 및 예시

- **README.md**: 플러그인 메인 문서
  - 한글 문서화
  - 설치 가이드
  - 사용법 및 예시

- **LICENSE**: 제한적 사용 라이선스
  - datamaker-kr organization 멤버 전용
  - 조직 내부 프로젝트 전용

#### 대상 프로젝트

- synapse-backend
- synapse-workspace
- synapse-annotator
- datamaker-kr organization 내 관련 프로젝트

---

## 이전 개발 버전

### [2026.0.4] - 2026-01-18

#### Documentation

- 각 스킬별 README.md 추가
  - skills/tdd-workflow/README.md: TDD 워크플로우 상세 가이드
  - skills/docs-manager/README.md: docs-manager 스킬 상세 가이드
  - skills/mermaid-expert/README.md: mermaid-expert 스킬 상세 가이드
  - skills/commit-message/README.md: commit-message 스킬 상세 가이드
- 메인 README.md 스킬 섹션 간소화 및 개별 README 링크 추가

### [2026.0.3] - 2026-01-18

#### Improved

- commit-message Skill: 언어 선택 기능 추가
  - 한글 (기본) 또는 영어로 커밋 메시지 작성 가능
  - 사용자 요청 시 언어 확인 단계 추가
  - 영어 커밋 메시지 규칙 및 예시 추가

#### Documentation

- CLAUDE.md: 영어 커밋 메시지 규칙 추가
- README.md: commit-message 스킬 언어 선택 기능 설명 업데이트
- skills/commit-message/SKILL.md: 영어 규칙 및 워크플로우 업데이트

### [2026.0.2] - 2026-01-18

#### Added

- commit-message Skill: 한글 커밋 메시지 작성 가이드 스킬 추가
  - 커밋 타입 자동 선택 (기능, 수정, 문서 등)
  - 72자 제한 적용
  - Co-Authored-By 자동 추가

#### Documentation

- CLAUDE.md: 커밋 메시지 규칙 문서화
- README.md: commit-message 스킬 섹션 추가

### [2026.0.1] - 2026-01-18 - 초기 개발 버전

#### Added

**스킬**:

- TDD Skill: Kent Beck의 TDD와 Tidy First 방법론
- docs-manager Skill: 적극적인 문서 모니터링
- mermaid-expert Skill: 색상 규칙을 따르는 다이어그램 생성

**커맨드**:

- `/update-pr-title`: PR 제목 자동 생성
- `/update-pr-desc`: 포괄적인 PR 설명 생성
- `/update-docs`: 수동 문서 업데이트

**기능**:

- GitHub MCP 서버 통합
- 이중 언어 지원 (한국어/영어)
- 라이트/다크 모드 호환 Mermaid 차트
- PR 템플릿 준수

**라이선스**:

- datamaker-kr organization 멤버 전용 제한적 사용 라이선스
- 조직 내부 프로젝트 전용

---

## 버전 관리 규칙

본 프로젝트는 Calendar Versioning (CalVer)을 따릅니다.

### 버전 형식: YYYY.Minor.Patch

- **YYYY**: 릴리스 연도 (예: 2026, 2027)
- **Minor**: 해당 연도 내 기능 추가 및 개선 (0부터 시작)
- **Patch**: 버그 수정 및 문서 업데이트 (0부터 시작)

### 버전 증가 규칙

**연도 (YYYY) 변경**:
- 새해가 되면 자동으로 변경
- Minor와 Patch는 0으로 리셋
- 예: `2026.3.5` → `2027.0.0`

**Minor 버전 증가**:
- 새로운 스킬 추가
- 새로운 커맨드 추가
- 기존 기능의 중요한 개선
- API 동작 변경 (Breaking Changes 포함 가능)
- 예: `2026.0.5` → `2026.1.0` (Patch는 0으로 리셋)

**Patch 버전 증가**:
- 버그 수정
- 문서 업데이트
- 성능 개선
- 내부 리팩토링 (동작 변경 없음)
- 예: `2026.1.0` → `2026.1.1`

---

[2026.2.1]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.2.0...v2026.2.1
[2026.2.0]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.1.3...v2026.2.0
[2026.1.3]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.1.2...v2026.1.3
[2026.1.2]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.1.1...v2026.1.2
[2026.1.1]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/releases/tag/v2026.1.1
[2026.0.4]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.0.3...v2026.0.4
[2026.0.3]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.0.2...v2026.0.3
[2026.0.2]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/compare/v2026.0.1...v2026.0.2
[2026.0.1]: https://github.com/datamaker-kr/platform-dev-team-claude-plugin/releases/tag/v2026.0.1
