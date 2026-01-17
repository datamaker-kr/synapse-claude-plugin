# Synapse Plugin Development Toolkit

Claude Code 플러그인으로 Synapse SDK 플러그인 개발을 지원합니다.

## 개요

이 플러그인은 synapse-sdk-v2의 CLI 도구를 활용하여 Synapse 플러그인 개발의 전체 워크플로우를 지원합니다:

- **플러그인 생성**: 새로운 Synapse 플러그인 스캐폴딩
- **개발 지원**: 액션, 설정, 컨텍스트 API 가이드
- **테스트 및 디버깅**: 로컬 테스트, 로그 스트리밍, 문제 해결
- **배포**: 검증 및 퍼블리싱

## 설치

Claude Code에서 플러그인 디렉토리를 지정하여 사용:

```bash
claude --plugin-dir /path/to/synapse-sdk-claude-plugin
```

## 기능

### Commands (슬래시 명령어)

| 명령어 | 설명 |
|--------|------|
| `/synapse-plugin:help` | 사용 가능한 모든 기능 안내 |
| `/synapse-plugin:create` | 새 Synapse 플러그인 생성 |
| `/synapse-plugin:config` | 플러그인 설정, 카테고리, 연결된 에이전트 조회 |
| `/synapse-plugin:test` | 로컬에서 액션 테스트 실행 |
| `/synapse-plugin:logs` | 실행 중인 작업의 로그 스트리밍 |
| `/synapse-plugin:debug` | 플러그인 문제 진단 및 해결 |
| `/synapse-plugin:update-config` | 코드 기반 메타데이터를 config.yaml에 동기화 |
| `/synapse-plugin:dry-run` | 배포 전 검증 |
| `/synapse-plugin:publish` | 플러그인 배포 |

### Skills (자동 활성화)

대화 맥락에 따라 자동으로 활성화되는 지식 제공:

| 스킬 | 트리거 키워드 |
|------|---------------|
| **action-development** | "액션 만들기", "@action", "BaseAction", "Pydantic" |
| **config-yaml-guide** | "config.yaml", "플러그인 설정", "액션 정의" |
| **plugin-execution** | "run_plugin", "ExecutionMode", "RayActorExecutor" |
| **result-schemas** | "TrainResult", "InferenceResult", "result_model" |
| **runtime-context-api** | "RuntimeContext", "ctx.", "set_progress", "log_message" |
| **specialized-actions** | "BaseTrainAction", "BaseExportAction", "BaseUploadAction" |
| **step-workflow** | "BaseStep", "StepRegistry", "Orchestrator" |

### Agents (자율 에이전트)

특정 상황에서 자동으로 호출되는 전문 에이전트:

| 에이전트 | 목적 |
|----------|------|
| **plugin-validator** | config.yaml, 엔트리포인트, 의존성 검증 |
| **troubleshooter** | 에러 분석 및 해결책 제안 |

## 빠른 시작

### 1. 새 플러그인 만들기

```
/synapse-plugin:create --name "My Plugin" --code my-plugin --category neural_net
```

### 2. 액션 개발하기

Claude에게 물어보세요:
- "BaseAction 클래스로 훈련 액션 만들어줘"
- "@action 데코레이터 사용법 알려줘"

### 3. 테스트하기

```
/synapse-plugin:test train --params '{"epochs": 10}'
```

### 4. 설정 동기화

```
/synapse-plugin:update-config
```

### 5. 검증 및 배포

```
/synapse-plugin:dry-run
/synapse-plugin:publish
```

## 요구사항

| 요구사항 | 최소 버전 | 비고 |
|----------|-----------|------|
| Claude Code | latest | - |
| Python | 3.12+ | `python3 --version` |
| synapse-sdk | latest | PyPI에서 설치 |
| uv (권장) 또는 pip | any | 패키지 관리자 |

### synapse-sdk 설치

```bash
# uv 사용 (권장 - 빠르고 안정적)
uv pip install synapse-sdk

# pip 사용 (대안)
pip install synapse-sdk
```

### uv 설치 (선택)

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 구조

```
synapse-sdk-claude-plugin/
├── .claude-plugin/
│   └── plugin.json          # 플러그인 매니페스트
├── commands/                 # 슬래시 명령어 (9개)
│   ├── help.md
│   ├── create.md
│   ├── config.md
│   ├── test.md
│   ├── logs.md
│   ├── debug.md
│   ├── update-config.md
│   ├── dry-run.md
│   └── publish.md
├── skills/                   # 자동 활성화 스킬 (7개)
│   ├── action-development/
│   ├── config-yaml-guide/
│   ├── plugin-execution/
│   ├── result-schemas/
│   ├── runtime-context-api/
│   ├── specialized-actions/
│   └── step-workflow/
├── agents/                   # 자율 에이전트 (2개)
│   ├── plugin-validator.md
│   └── troubleshooter.md
└── README.md
```

## 라이선스

MIT
