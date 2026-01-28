# Synapse Plugin Marketplace

Synapse 제품군 개발을 위한 공식 Claude Code 플러그인 마켓플레이스입니다.

## 개요

이 마켓플레이스는 Synapse 제품군 개발에 필요한 Claude Code 플러그인의 중앙 등록소입니다.

### 지원 제품

- synapse-backend
- synapse-annotator
- synapse-workspace
- synapse-sdk
- synapse-agent

## 빠른 시작

### 1. 마켓플레이스 추가

```bash
/plugin marketplace add datamaker-kr/synapse-claude-marketplace
```

### 2. 사용 가능한 플러그인 탐색

```bash
/plugin > Discover
```

### 3. 플러그인 설치

```bash
# Synapse SDK 플러그인 개발 도구
/plugin install synapse-plugin-helper@synapse-marketplace

# 개발팀 공통 도구 (TDD, 문서 관리, PR 자동화)
/plugin install platform-dev-team-common@synapse-marketplace
```

## 사용 가능한 플러그인

| 플러그인 | 설명 | 버전 | 카테고리 |
|---------|------|------|----------|
| [synapse-plugin-helper](#synapse-plugin-helper) | Synapse SDK 플러그인 개발 도구 | 1.0.0 | development |
| [platform-dev-team-common](#platform-dev-team-common) | TDD, 문서 관리, PR 자동화 | 1.0.0 | development |

---

## synapse-plugin-helper

Synapse SDK 플러그인 개발을 위한 Claude Code 도구입니다.

### 명령어 (9개)

| 명령어 | 설명 |
|--------|------|
| `/synapse-plugin-helper:help` | 사용 가능한 모든 기능 안내 |
| `/synapse-plugin-helper:create` | 새 Synapse 플러그인 생성 |
| `/synapse-plugin-helper:config` | 플러그인 설정, 카테고리, 연결된 에이전트 조회 |
| `/synapse-plugin-helper:test` | 로컬에서 액션 테스트 실행 |
| `/synapse-plugin-helper:logs` | 실행 중인 작업의 로그 스트리밍 |
| `/synapse-plugin-helper:debug` | 플러그인 문제 진단 및 해결 |
| `/synapse-plugin-helper:update-config` | 코드 기반 메타데이터를 config.yaml에 동기화 |
| `/synapse-plugin-helper:dry-run` | 배포 전 검증 |
| `/synapse-plugin-helper:publish` | 플러그인 배포 |

### 스킬 (7개)

| 스킬 | 트리거 키워드 |
|------|---------------|
| **action-development** | "액션 만들기", "@action", "BaseAction", "Pydantic" |
| **config-yaml-guide** | "config.yaml", "플러그인 설정", "액션 정의" |
| **plugin-execution** | "run_plugin", "ExecutionMode", "RayActorExecutor" |
| **result-schemas** | "TrainResult", "InferenceResult", "result_model" |
| **runtime-context-api** | "RuntimeContext", "ctx.", "set_progress", "log_message" |
| **specialized-actions** | "BaseTrainAction", "BaseExportAction", "BaseUploadAction" |
| **step-workflow** | "BaseStep", "StepRegistry", "Orchestrator" |

### 에이전트 (2개)

| 에이전트 | 목적 |
|----------|------|
| **plugin-validator** | config.yaml, 엔트리포인트, 의존성 검증 |
| **troubleshooter** | 에러 분석 및 해결책 제안 |

---

## platform-dev-team-common

개발팀 공통 도구로 TDD, 문서 관리, PR 자동화를 지원합니다.

### 명령어 (4개)

| 명령어 | 설명 |
|--------|------|
| `/platform-dev-team-common:update-pr-title` | PR 제목 자동 생성 |
| `/platform-dev-team-common:update-pr-desc` | PR 설명 및 Mermaid 다이어그램 자동 생성 |
| `/platform-dev-team-common:update-docs` | 문서 업데이트 분석 및 제안 |
| `/platform-dev-team-common:add-changelog` | CHANGELOG 항목 추가 |

### 스킬 (6개)

| 스킬 | 설명 |
|------|------|
| **tdd-workflow** | Kent Beck의 TDD 방법론 및 Tidy First 원칙 가이드 |
| **docs-analyzer** | 코드 변경 분석 및 문서 갭 식별 |
| **docs-bootstrapper** | 프로젝트 문서 구조 초기화 |
| **mermaid-expert** | Light/Dark 모드 호환 Mermaid 다이어그램 생성 |
| **commit-with-message** | 한국어/영어 이중 언어 커밋 메시지 규칙 |
| **changelog-manager** | Keep a Changelog 형식의 변경 로그 관리 |

### 에이전트 (3개)

| 에이전트 | 목적 |
|----------|------|
| **docs-manager** | 문서 자동 분석 및 업데이트 제안 |
| **planner** | 구현 계획 설계 |
| **update-pr** | PR 자동 업데이트 |

---

## 요구 사항

### 필수 사항

| 항목 | 확인 명령어 | 최소 버전 |
|------|------------|-----------|
| Claude Code | `claude --version` | v2.1.0+ |
| GITHUB_TOKEN | `echo $GITHUB_TOKEN` | - |

### synapse-plugin-helper 추가 요구 사항

| 항목 | 확인 명령어 | 최소 버전 | 비고 |
|------|------------|-----------|------|
| Python | `python3 --version` | 3.12+ | 필수 |
| synapse-sdk | `synapse --version` | latest | PyPI에서 설치 |
| uv (권장) | `uv --version` | any | 패키지 관리자 |

### Personal Access Token 설정

Claude Code 마켓플레이스에서 private 레포지토리를 사용하려면 GitHub 토큰 설정이 필요합니다.

```bash
# ~/.zshrc 또는 ~/.bashrc에 추가
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
source ~/.zshrc
```

## 문제 해결

### GITHUB_TOKEN 인증 실패

**증상**: 마켓플레이스 추가 시 "authentication failed" 오류

**해결**:
```bash
echo $GITHUB_TOKEN  # 토큰 확인
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
```

### 조직 접근 권한 오류

**증상**: `404 Not Found` 또는 `Repository not found`

**해결**:
1. [datamaker-kr](https://github.com/datamaker-kr) 조직 멤버인지 확인
2. Personal Access Token의 `repo` 스코프 확인

---

## 기존 사용자 마이그레이션

v1.0.0부터 synapse-plugin 명령어 prefix가 변경되었습니다:

| 이전 | 현재 |
|------|------|
| `/synapse-plugin:help` | `/synapse-plugin-helper:help` |
| `/synapse-plugin:create` | `/synapse-plugin-helper:create` |
| `/synapse-plugin:config` | `/synapse-plugin-helper:config` |
| `/synapse-plugin:test` | `/synapse-plugin-helper:test` |
| `/synapse-plugin:logs` | `/synapse-plugin-helper:logs` |
| `/synapse-plugin:debug` | `/synapse-plugin-helper:debug` |
| `/synapse-plugin:update-config` | `/synapse-plugin-helper:update-config` |
| `/synapse-plugin:dry-run` | `/synapse-plugin-helper:dry-run` |
| `/synapse-plugin:publish` | `/synapse-plugin-helper:publish` |

---

## 라이선스

MIT

## 지원

- GitHub Issues: [이슈 생성](https://github.com/datamaker-kr/synapse-claude-marketplace/issues)
- 조직: [datamaker-kr](https://github.com/datamaker-kr)
