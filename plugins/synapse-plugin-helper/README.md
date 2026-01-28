# Synapse Plugin Helper

Synapse SDK 플러그인 개발을 위한 Claude Code 도구입니다. 플러그인 생성부터 테스트, 디버깅, 배포까지 전체 개발 워크플로우를 지원합니다.

> **📌 라이선스**: MIT

## 개요

이 플러그인은 synapse-sdk-v2의 CLI 도구를 활용하여 Synapse 플러그인 개발의 전체 워크플로우를 지원합니다:

- **플러그인 생성**: 새로운 Synapse 플러그인 스캐폴딩
- **개발 지원**: 액션, 설정, 컨텍스트 API 가이드
- **테스트 및 디버깅**: 로컬 테스트, 로그 스트리밍, 문제 해결
- **배포**: 검증 및 퍼블리싱

**소속 조직**: [datamaker-kr](https://github.com/datamaker-kr)

## 설치

### 사전 요구사항

| 항목 | 확인 명령어 | 최소 버전 | 비고 |
|------|------------|-----------|------|
| Claude Code | `claude --version` | v2.1.0+ | - |
| Python | `python3 --version` | 3.12+ | 필수 |
| synapse-sdk | `synapse --version` | latest | PyPI에서 설치 |
| uv (권장) | `uv --version` | any | 패키지 관리자 |

### synapse-sdk 설치

```bash
# uv 사용 (권장 - 빠르고 안정적)
uv pip install synapse-sdk

# pip 사용 (대안)
pip install synapse-sdk
```

### 마켓플레이스를 통한 설치

```bash
# 마켓플레이스 추가
/plugin marketplace add datamaker-kr/synapse-claude-marketplace

# 플러그인 설치
/plugin install synapse-plugin-helper@synapse-marketplace
```

---

## 명령어

### /synapse-plugin-helper:help

사용 가능한 모든 기능을 안내합니다.

```bash
/synapse-plugin-helper:help
```

### /synapse-plugin-helper:create

새 Synapse 플러그인을 생성합니다.

```bash
/synapse-plugin-helper:create --name "My Plugin" --code my-plugin --category neural_net
```

**옵션**:
- `--name`: 플러그인 표시 이름
- `--code`: 플러그인 코드 (kebab-case)
- `--path`: 생성 경로 (기본: 현재 디렉토리)
- `--category`: 카테고리 (neural_net, export, upload, smart_tool, custom)
- `--yes`: 확인 없이 진행

### /synapse-plugin-helper:config

플러그인 설정, 카테고리, 연결된 에이전트를 조회합니다.

```bash
/synapse-plugin-helper:config
```

### /synapse-plugin-helper:test

로컬에서 액션을 테스트합니다.

```bash
/synapse-plugin-helper:test train --params '{"epochs": 10}'
```

### /synapse-plugin-helper:logs

실행 중인 작업의 로그를 스트리밍합니다.

```bash
/synapse-plugin-helper:logs --job-id <job-id>
```

### /synapse-plugin-helper:debug

플러그인 문제를 진단하고 해결책을 제안합니다.

```bash
/synapse-plugin-helper:debug
```

### /synapse-plugin-helper:update-config

코드 기반 메타데이터를 config.yaml에 동기화합니다.

```bash
/synapse-plugin-helper:update-config
```

### /synapse-plugin-helper:dry-run

배포 전 플러그인을 검증합니다.

```bash
/synapse-plugin-helper:dry-run
```

### /synapse-plugin-helper:publish

플러그인을 Synapse 플랫폼에 배포합니다.

```bash
/synapse-plugin-helper:publish
```

---

## 스킬

대화 맥락에 따라 자동으로 활성화되는 지식을 제공합니다.

### action-development

액션 개발 패턴을 안내합니다.

- **트리거**: "액션 만들기", "@action", "BaseAction", "Pydantic"
- **내용**: 함수 기반/클래스 기반 액션 패턴, 데코레이터, Pydantic 스키마

### config-yaml-guide

config.yaml 작성 방법을 안내합니다.

- **트리거**: "config.yaml", "플러그인 설정", "액션 정의"
- **내용**: 필드 설명, 실행 모드, 런타임 환경

### plugin-execution

플러그인 실행 메커니즘을 설명합니다.

- **트리거**: "run_plugin", "ExecutionMode", "RayActorExecutor"
- **내용**: 플러그인 디스커버리, 실행기, run_plugin API

### result-schemas

결과 스키마 작성법을 안내합니다.

- **트리거**: "TrainResult", "InferenceResult", "result_model"
- **내용**: TrainResult, InferenceResult, 기타 결과 타입

### runtime-context-api

RuntimeContext API 사용법을 설명합니다.

- **트리거**: "RuntimeContext", "ctx.", "set_progress", "log_message"
- **내용**: 로깅 패턴, 진행률 추적, 특화된 컨텍스트

### specialized-actions

특화된 액션 클래스를 안내합니다.

- **트리거**: "BaseTrainAction", "BaseExportAction", "BaseUploadAction"
- **내용**: 학습, 추론, 내보내기, 업로드 액션

### step-workflow

Step 기반 워크플로우를 설명합니다.

- **트리거**: "BaseStep", "StepRegistry", "Orchestrator"
- **내용**: Step 클래스, 컨텍스트, 오케스트레이터

---

## 에이전트

특정 상황에서 자동으로 호출되는 전문 에이전트입니다.

### plugin-validator

플러그인 구성을 검증합니다.

- **활성화**: "플러그인 검증", "config.yaml 확인"
- **기능**: config.yaml, 엔트리포인트, 의존성, Pydantic 스키마 검증

### troubleshooter

에러를 분석하고 해결책을 제안합니다.

- **활성화**: 에러 발생 시 자동
- **기능**: 에러 메시지 분석, 일반적인 문제 해결책 제안

---

## 디렉토리 구조

```
synapse-plugin-helper/
├── plugin.json              # 플러그인 매니페스트
├── agents/
│   ├── plugin-validator.md  # 검증 에이전트
│   └── troubleshooter.md    # 문제 해결 에이전트
├── commands/
│   ├── help.md
│   ├── create.md
│   ├── config.md
│   ├── test.md
│   ├── logs.md
│   ├── debug.md
│   ├── update-config.md
│   ├── dry-run.md
│   └── publish.md
├── skills/
│   ├── action-development/
│   ├── config-yaml-guide/
│   ├── plugin-execution/
│   ├── result-schemas/
│   ├── runtime-context-api/
│   ├── specialized-actions/
│   └── step-workflow/
└── README.md
```

---

## 빠른 시작

### 1. 새 플러그인 만들기

```bash
/synapse-plugin-helper:create --name "My Plugin" --code my-plugin --category neural_net
```

### 2. 액션 개발하기

Claude에게 물어보세요:
- "BaseAction 클래스로 훈련 액션 만들어줘"
- "@action 데코레이터 사용법 알려줘"

### 3. 테스트하기

```bash
/synapse-plugin-helper:test train --params '{"epochs": 10}'
```

### 4. 설정 동기화

```bash
/synapse-plugin-helper:update-config
```

### 5. 검증 및 배포

```bash
/synapse-plugin-helper:dry-run
/synapse-plugin-helper:publish
```

---

## 문제 해결

### synapse-sdk 명령어 오류

**증상**: `synapse: command not found`

**해결**:
```bash
pip install synapse-sdk
synapse --version
```

### Python 버전 불일치

**증상**: `Python 3.12+ required`

**해결**:
```bash
# macOS
brew install python@3.12

# Ubuntu
sudo apt install python3.12
```

### 인증 오류

**증상**: Synapse 플랫폼 인증 실패

**해결**:
```bash
synapse doctor  # 연결 상태 확인
synapse login   # 재로그인
```

---

## 버전

- **현재 버전**: 1.0.0
- **버전 관리**: Semantic Versioning (SemVer)

## 라이선스

MIT

## 지원

- GitHub Issues: [이슈 생성](https://github.com/datamaker-kr/synapse-claude-marketplace/issues)
- 조직: [datamaker-kr](https://github.com/datamaker-kr)
