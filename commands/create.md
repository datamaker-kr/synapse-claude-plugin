---
description: Create a new Synapse plugin with guided setup
argument-hint: [--name <plugin-name>] [--code <plugin-code>] [--path ./path] [--category neural_net|export|upload|smart_tool|custom] [--yes]
allowed-tools: ["Bash", "Read", "Write", "AskUserQuestion", "TodoWrite"]
---

# Create Synapse Plugin

Create a new Synapse plugin using the `synapse plugin create` CLI command.

**Arguments:** $ARGUMENTS

## Workflow

### Step 1: Environment Detection

**CRITICAL**: Before proceeding, verify the development environment.

#### 1.1 Check Package Manager Availability

Run these checks in order:

```bash
# Check uv (priority 1)
uv --version 2>/dev/null && echo "UV_AVAILABLE=true" || echo "UV_AVAILABLE=false"

# Check pip (priority 2)
pip --version 2>/dev/null && echo "PIP_AVAILABLE=true" || echo "PIP_AVAILABLE=false"

# Check Python version (requires 3.12+)
python3 --version 2>/dev/null || python --version 2>/dev/null
```

**Package Manager Priority:**
1. **uv** (추천) - 빠르고 안정적인 패키지 관리
2. **pip** - 기본 Python 패키지 관리자

#### 1.2 Check Python Version

synapse-sdk requires Python 3.12 or higher:

```bash
python3 -c "import sys; v=sys.version_info; print(f'Python {v.major}.{v.minor}.{v.micro}'); exit(0 if v >= (3,12) else 1)"
```

If Python < 3.12:
```
⚠️ Python 3.12 이상이 필요합니다.
현재 버전: [version]

설치 방법:
- macOS: brew install python@3.11
- Ubuntu: sudo apt install python3.11
- Windows: https://python.org/downloads/
```

#### 1.3 Check synapse-sdk Installation

```bash
# Check if synapse CLI is available
synapse --version 2>/dev/null
```

### Step 2: Install synapse-sdk (if needed)

If `synapse` command not found, guide installation:

#### Using uv (권장)

```bash
# uv가 설치되어 있는 경우
uv pip install synapse-sdk
```

또는 uv 가상환경 사용:
```bash
uv venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
uv pip install synapse-sdk
```

#### Using pip (대안)

```bash
# pip 사용 시
pip install synapse-sdk
```

또는 가상환경 사용:
```bash
python3 -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install synapse-sdk
```

**Display to user:**
```
╔══════════════════════════════════════════════════╗
║        SYNAPSE SDK 설치 필요                      ║
╠══════════════════════════════════════════════════╣
║ synapse-sdk가 설치되어 있지 않습니다.             ║
║                                                  ║
║ 설치 방법 (택 1):                                ║
║                                                  ║
║ [권장] uv 사용:                                  ║
║   uv pip install synapse-sdk                    ║
║                                                  ║
║ [대안] pip 사용:                                 ║
║   pip install synapse-sdk                       ║
╠══════════════════════════════════════════════════╣
║ 설치 후 다시 실행해주세요.                        ║
╚══════════════════════════════════════════════════╝
```

Ask user: "synapse-sdk를 지금 설치할까요? (uv/pip/취소)"

If user agrees:
- Use uv if available, otherwise pip
- Run installation command
- Verify installation with `synapse --version`

### Step 3: Parse Arguments

Extract options from arguments:
- `--name` - Plugin display name (optional, will prompt if not provided)
- `--code` - Plugin code/slug (optional, will prompt if not provided)
- `--path` - Target directory (default: current directory)
- `--category` - Plugin category (default: custom)
- `--yes` - Skip confirmation prompts

### Step 4: Gather Information

If plugin name/code not provided in arguments, ask user:

```
What should the plugin be named? (Display name, e.g., My Awesome Plugin)
What should the plugin code be? (kebab-case, e.g., my-awesome-plugin)
```

### Step 5: Execute CLI Command

Run the Synapse CLI to create the plugin structure:

```bash
synapse plugin create --name [plugin-name] --code [plugin-code] --path [path] --category [category]
```

**Available categories:**
| Category | Description |
|----------|-------------|
| `neural_net` | ML model training/inference |
| `export` | Data format conversion |
| `upload` | External data import |
| `smart_tool` | AI-assisted annotation |
| `pre_annotation` | Pre-labeling processing |
| `post_annotation` | Post-labeling processing |
| `data_validation` | Data quality checks |
| `custom` | User-defined |

### Step 6: Verify Creation

After running the command:
1. Check that `config.yaml` was created
2. Verify the plugin directory name (`synapse-[code]-plugin`)
3. List the generated files

```bash
ls -la [plugin-path]/  # e.g., ./synapse-[code]-plugin/
cat [plugin-path]/config.yaml
```

### Step 7: Next Steps

Inform user about:
- How to edit `config.yaml`
- How to create actions
- How to test with `/synapse-plugin:test`

```
╔══════════════════════════════════════════════════╗
║        플러그인 생성 완료! 🎉                     ║
╠══════════════════════════════════════════════════╣
║ 다음 단계:                                       ║
║                                                  ║
║ 1. config.yaml 수정                              ║
║    - 액션 정의 추가                              ║
║    - 메타데이터 설정                             ║
║                                                  ║
║ 2. 액션 코드 작성                                ║
║    "BaseAction 만드는 법 알려줘" 질문하기        ║
║                                                  ║
║ 3. 의존성 설치                                   ║
║    uv sync (또는 pip install -r requirements.txt)║
║                                                  ║
║ 4. 로컬 테스트                                   ║
║    /synapse-plugin:test [action-name]           ║
║                                                  ║
║ 5. 배포 전 검증                                  ║
║    /synapse-plugin:dry-run                      ║
╚══════════════════════════════════════════════════╝
```

## Error Handling

### uv not installed

```
uv가 설치되어 있지 않습니다.

uv 설치 방법:
- macOS/Linux: curl -LsSf https://astral.sh/uv/install.sh | sh
- Windows: powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
- pip: pip install uv

또는 pip을 직접 사용할 수 있습니다.
```

### pip not working

```
pip을 찾을 수 없습니다.

해결 방법:
- Python 재설치 (pip 포함 확인)
- python3 -m ensurepip --upgrade
```

### Directory already exists

```
Directory already exists. Choose:
1. Use a different path
2. Overwrite existing files (--yes)
```

### Network error during installation

```
패키지 다운로드 실패.

확인사항:
- 인터넷 연결 확인
- 프록시 설정 확인 (기업 환경)
- PyPI 접근 가능 여부: pip index versions synapse-sdk
```
