# Claude Code on the web

> Source: https://code.claude.com/docs/en/claude-code-on-the-web

Claude Code on the web is currently in research preview.

## What is Claude Code on the web?

Claude Code on the web lets developers kick off Claude Code from the Claude app. This is perfect for:

- **Answering questions:** Ask about code architecture and how features are implemented
- **Bug fixes and routine tasks:** Well-defined tasks that don't require frequent steering
- **Parallel work:** Tackle multiple bug fixes in parallel
- **Repositories not on your local machine:** Work on code you don't have checked out locally
- **Backend changes:** Where Claude Code can write tests and then write code to pass those tests

Claude Code is also available on the Claude iOS app for kicking off tasks on the go and monitoring work in progress.

You can move between local and remote development: send tasks from your terminal to run on the web with the `&` prefix, or teleport web sessions back to your terminal to continue locally.

## Who can use Claude Code on the web?

Claude Code on the web is available in research preview to:
- Pro users
- Max users
- Team premium seat users
- Enterprise premium seat users

## Getting started

1. Visit claude.ai/code
2. Connect your GitHub account
3. Install the Claude GitHub app in your repositories
4. Select your default environment
5. Submit your coding task
6. Review changes and create a pull request in GitHub

## How it works

When you start a task on Claude Code on the web:

1. **Repository cloning:** Your repository is cloned to an Anthropic-managed virtual machine
2. **Environment setup:** Claude prepares a secure cloud environment with your code
3. **Network configuration:** Internet access is configured based on your settings
4. **Task execution:** Claude analyzes code, makes changes, runs tests, and checks its work
5. **Completion:** You're notified when finished and can create a PR with the changes
6. **Results:** Changes are pushed to a branch, ready for pull request creation

## Moving tasks between web and terminal

You can start tasks on the web and continue them in your terminal, or send tasks from your terminal to run on the web. Web sessions persist even if you close your laptop, and you can monitor them from anywhere including the Claude iOS app.

> Session handoff is one-way: you can pull web sessions into your terminal, but you can't push an existing terminal session to the web. The `&` prefix creates a new web session with your current conversation context.

### From terminal to web

Start a message with `&` inside Claude Code to send a task to run on the web:

```
& Fix the authentication bug in src/auth/login.ts
```

This creates a new web session on claude.ai with your current conversation context. The task runs in the cloud while you continue working locally. Use `/tasks` to check progress, or open the session on claude.ai or the Claude iOS app to interact directly. From there you can steer Claude, provide feedback, or answer questions just like any other conversation.

You can also start a web session directly from the command line:

```bash
claude --remote "Fix the authentication bug in src/auth/login.ts"
```

### Tips for background tasks

**Plan locally, execute remotely:**

For complex tasks, start Claude in plan mode to collaborate on the approach before sending work to the web:

```bash
claude --permission-mode plan
```

In plan mode, Claude can only read files and explore the codebase. Once you're satisfied with the plan, send it to the web for autonomous execution:

```
& Execute the migration plan we discussed
```

This pattern gives you control over the strategy while letting Claude execute autonomously in the cloud.

**Run tasks in parallel:**

Each `&` command creates its own web session that runs independently. You can kick off multiple tasks and they'll all run simultaneously in separate sessions:

```
& Fix the flaky test in auth.spec.ts
& Update the API documentation
& Refactor the logger to use structured output
```

Monitor all sessions with `/tasks`. When a session completes, you can create a PR from the web interface or teleport the session to your terminal to continue working.

### From web to terminal

There are several ways to pull a web session into your terminal:

- **Using /teleport:** From within Claude Code, run `/teleport` (or `/tp`) to see an interactive picker of your web sessions. If you have uncommitted changes, you'll be prompted to stash them first.
- **Using --teleport:** From the command line, run `claude --teleport` for an interactive session picker, or `claude --teleport <session-id>` to resume a specific session directly.
- **From /tasks:** Run `/tasks` to see your background sessions, then press `t` to teleport into one
- **From the web interface:** Click "Open in CLI" to copy a command you can paste into your terminal

When you teleport a session, Claude verifies you're in the correct repository, fetches and checks out the branch from the remote session, and loads the full conversation history into your terminal.

### Requirements for teleporting

| Requirement | Details |
|-------------|---------|
| Clean git state | Your working directory must have no uncommitted changes. Teleport prompts you to stash changes if needed. |
| Correct repository | You must run `--teleport` from a checkout of the same repository, not a fork. |
| Branch available | The branch from the web session must have been pushed to the remote. Teleport automatically fetches and checks it out. |
| Same account | You must be authenticated to the same Claude.ai account used in the web session. |

## Cloud environment

### Default image

We build and maintain a universal image with common toolchains and language ecosystems pre-installed. This image includes:

- Popular programming languages and runtimes
- Common build tools and package managers
- Testing frameworks and linters

### Checking available tools

To see what's pre-installed in your environment, ask Claude Code to run:

```
check-tools
```

This command displays:
- Programming languages and their versions
- Available package managers
- Installed development tools

### Language-specific setups

The universal image includes pre-configured environments for:

- **Python:** Python 3.x with pip, poetry, and common scientific libraries
- **Node.js:** Latest LTS versions with npm, yarn, pnpm, and bun
- **Ruby:** Versions 3.1.6, 3.2.6, 3.3.6 (default: 3.3.6) with gem, bundler, and rbenv for version management
- **PHP:** Version 8.4.14
- **Java:** OpenJDK with Maven and Gradle
- **Go:** Latest stable version with module support
- **Rust:** Rust toolchain with cargo
- **C++:** GCC and Clang compilers

### Databases

The universal image includes the following databases:

- **PostgreSQL:** Version 16
- **Redis:** Version 7.0

## Environment configuration

When you start a session in Claude Code on the web, here's what happens under the hood:

1. **Environment preparation:** We clone your repository and run any configured Claude hooks for initialization. The repo will be cloned with the default branch on your GitHub repo. If you would like to check out a specific branch, you can specify that in the prompt.

2. **Network configuration:** We configure internet access for the agent. Internet access is limited by default, but you can configure the environment to have no internet or full internet access based on your needs.

3. **Claude Code execution:** Claude Code runs to complete your task, writing code, running tests, and checking its work. You can guide and steer Claude throughout the session via the web interface. Claude respects context you've defined in your CLAUDE.md.

4. **Outcome:** When Claude completes its work, it will push the branch to remote. You will be able to create a PR for the branch.

Claude operates entirely through the terminal and CLI tools available in the environment. It uses the pre-installed tools in the universal image and any additional tools you install through hooks or dependency management.

**To add a new environment:**
Select the current environment to open the environment selector, and then select "Add environment". This will open a dialog where you can specify the environment name, network access level, and any environment variables you want to set.

**To update an existing environment:**
Select the current environment, to the right of the environment name, and select the settings button. This will open a dialog where you can update the environment name, network access, and environment variables.

**To select your default environment from the terminal:**
If you have multiple environments configured, run `/remote-env` to choose which one to use when starting web sessions from your terminal with `&` or `--remote`. With a single environment, this command shows your current configuration.

Environment variables must be specified as key-value pairs, in .env format. For example:
```
API_KEY=your_api_key
DEBUG=true
```

### Dependency management

Configure automatic dependency installation using SessionStart hooks. This can be configured in your repository's `.claude/settings.json` file:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/scripts/install_pkgs.sh"
          }
        ]
      }
    ]
  }
}
```

Create the corresponding script at `scripts/install_pkgs.sh`:

```bash
#!/bin/bash
npm install
pip install -r requirements.txt
exit 0
```

Make it executable: `chmod +x scripts/install_pkgs.sh`

### Local vs remote execution

By default, all hooks execute both locally and in remote (web) environments. To run a hook only in one environment, check the `CLAUDE_CODE_REMOTE` environment variable in your hook script.

```bash
#!/bin/bash
# Example: Only run in remote environments
if [ "$CLAUDE_CODE_REMOTE" != "true" ]; then
  exit 0
fi

npm install
pip install -r requirements.txt
```

### Persisting environment variables

SessionStart hooks can persist environment variables for subsequent bash commands by writing to the file specified in the `CLAUDE_ENV_FILE` environment variable. For details, see SessionStart hooks in the hooks reference.

## Network access and security

### Network policy

#### GitHub proxy

For security, all GitHub operations go through a dedicated proxy service that transparently handles all git interactions. Inside the sandbox, the git client authenticates using a custom-built scoped credential. This proxy:

- Manages GitHub authentication securely - the git client uses a scoped credential inside the sandbox, which the proxy verifies and translates to your actual GitHub authentication token
- Restricts git push operations to the current working branch for safety
- Enables seamless cloning, fetching, and PR operations while maintaining security boundaries

#### Security proxy

Environments run behind an HTTP/HTTPS network proxy for security and abuse prevention purposes. All outbound internet traffic passes through this proxy, which provides:

- Protection against malicious requests
- Rate limiting and abuse prevention
- Content filtering for enhanced security

### Access levels

By default, network access is limited to allowlisted domains. You can configure custom network access, including disabling network access.

### Default allowed domains

When using "Limited" network access, the following domains are allowed by default:

- **Anthropic Services:** api.anthropic.com, statsig.anthropic.com, claude.ai
- **Version Control:** github.com, gitlab.com, bitbucket.org and related subdomains
- **Container Registries:** Docker Hub, GCR, GHCR, MCR
- **Cloud Platforms:** Google Cloud, Azure, AWS, Oracle
- **Package Managers:** npm, PyPI, RubyGems, crates.io, Go proxy, Maven, Gradle, and more
- **Linux Distributions:** Ubuntu repositories
- **Development Tools:** Kubernetes, HashiCorp, Anaconda, Apache, Node.js

### Security best practices for customized network access

- **Principle of least privilege:** Only enable the minimum network access required
- **Audit regularly:** Review allowed domains periodically
- **Use HTTPS:** Always prefer HTTPS endpoints over HTTP

## Security and isolation

Claude Code on the web provides strong security guarantees:

- **Isolated virtual machines:** Each session runs in an isolated, Anthropic-managed VM
- **Network access controls:** Network access is limited by default, and can be disabled

> When running with network access disabled, Claude Code is allowed to communicate with the Anthropic API which may still allow data to exit the isolated Claude Code VM.

- **Credential protection:** Sensitive credentials (such as git credentials or signing keys) are never inside the sandbox with Claude Code. Authentication is handled through a secure proxy using scoped credentials
- **Secure analysis:** Code is analyzed and modified within isolated VMs before creating PRs

## Pricing and rate limits

Claude Code on the web shares rate limits with all other Claude and Claude Code usage within your account. Running multiple tasks in parallel will consume more rate limits proportionately.

## Limitations

- **Repository authentication:** You can only move sessions from web to local when you are authenticated to the same account
- **Platform restrictions:** Claude Code on the web only works with code hosted in GitHub. GitLab and other non-GitHub repositories cannot be used with cloud sessions

## Best practices

- **Use Claude Code hooks:** Configure SessionStart hooks to automate environment setup and dependency installation.
- **Document requirements:** Clearly specify dependencies and commands in your CLAUDE.md file. If you have an AGENTS.md file, you can source it in your CLAUDE.md using `@AGENTS.md` to maintain a single source of truth.

## Related resources

- Hooks configuration
- Settings reference
- Security
- Data usage
