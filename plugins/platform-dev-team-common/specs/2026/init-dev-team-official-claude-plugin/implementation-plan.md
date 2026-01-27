# Implementation Plan: Platform Dev Team Claude Plugin

## Overview
Create an official Claude plugin for the Platform Development Team that provides TDD guidance, documentation management, and PR management capabilities. This plugin will be used across synapse-backend, synapse-workspace, and synapse-annotator projects.

## User Decisions
- **Git Tracking:** Skills and commands directories will be tracked in git
- **GitHub Integration:** Use GitHub MCP server (not gh CLI)
- **Distribution:** Plugin marketplace with Claude Code
- **docs-manager Behavior:** Proactive - auto-suggest documentation updates after code changes

## Requirements Summary

### Skills to Create
1. **TDD Skill** - Enforces Test-Driven Development principles (Kent Beck's methodology)
2. **docs-manager Skill** - Manages documentation updates and Mermaid chart generation
3. **mermaid-expert Skill** - Helper skill for generating proper Mermaid charts

### Commands to Create
1. **update-pr-title** - Auto-generates PR titles with options for language and PR number
2. **update-pr-desc** - Updates PR descriptions with comprehensive template compliance
3. **update-docs** - Triggered by docs-manager skill to update documentation

## Implementation Steps

### Step 1: Update plugin.json Configuration

**File:** `.claude-plugin/plugin.json`

Update with proper metadata and register all skills/commands:

```json
{
  "name": "platform-dev-team-claude-plugin",
  "version": "0.1.0",
  "description": "Platform Dev Team official Claude plugin - TDD, documentation, and PR management",
  "author": {
    "name": "Platform Dev Team"
  },
  "license": "MIT",
  "keywords": ["tdd", "documentation", "pr-management", "mermaid", "dev-team"],
  "commands": [
    "./commands/update-pr-title.md",
    "./commands/update-pr-desc.md",
    "./commands/update-docs.md"
  ],
  "skills": "./skills/"
}
```

### Step 2: Create TDD Workflow

**File:** `skills/tdd-workflow/SKILL.md`

- Use content from specs (lines 28-104 of init-dev-team-official-claude-plugin.specs.md)
- Add YAML frontmatter with:
  - `name: tdd-workflow`
  - `description: Guides development following Kent Beck's TDD and Tidy First principles`
  - `allowed-tools: Read, Bash(git:*), Grep, Edit, Write`
  - `user-invocable: true`
- Format the provided content as proper skill instructions
- Include Red → Green → Refactor cycle
- Include Tidy First approach (structural vs behavioral changes)
- Include commit discipline rules

### Step 3: Create docs-manager Skill

**File:** `skills/docs-manager/SKILL.md`

**Capabilities:**
- Analyze repository documentation structure
- Detect mismatches between docs and current code changes
- Auto-update related documentation after implementation
- Supply update-docs command functionality
- Generate Mermaid charts following conventions

**File:** `skills/docs-manager/mermaid-guidelines.md`

- Copy content from `specs/2026/init-dev-team-official-claude-plugin/mermaid-chart-convention.md`
- Reference this from main SKILL.md for chart generation guidelines

**Skill Instructions (Proactive Mode):**
- **Automatically monitor** code changes during development sessions
- When files are edited, proactively identify affected documentation
- Analyze the nature of changes (API, models, architecture, features)
- **Auto-suggest** documentation updates with specific recommendations
- Generate or update Mermaid charts for visualizing changes
- Present suggestions to user for approval before making changes
- Always use Mermaid charts for visualizing architecture/flows
- Follow color palette: Blue #3b82f6, Green #10b981, Yellow #f59e0b, Red #ef4444
- Ensure light/dark mode compatibility

**Trigger Conditions:**
- After completing feature implementation
- After refactoring that affects architecture
- When API endpoints are added/modified
- When database models change
- When new major components are introduced

### Step 4: Create Mermaid Expert Skill (Helper)

**File:** `skills/mermaid-expert/SKILL.md`

- Specialized skill for generating Mermaid diagrams
- Enforce color conventions from mermaid-chart-convention.md
- Provide templates for common diagram types:
  - API endpoint flows (Client → API → ViewSet → Service → DB)
  - ER diagrams for database schemas
  - Process flowcharts with decision points
  - Sequence diagrams for multi-component interactions
- Always include style directives for all nodes
- Never use pure black (#000000) or pure white (#FFFFFF)

### Step 5: Create update-pr-title Command

**File:** `commands/update-pr-title.md`

**Content:** Use specs from `specs/2026/init-dev-team-official-claude-plugin/update-pr-title.md`

**Implementation approach:**
1. Parse arguments: `--pr <number>`, `--lang <ko|eng>`
2. If no PR number provided, detect current branch and find associated PR using git and GitHub MCP
3. Use GitHub MCP server tools to get PR information:
   - `get_pull_request` - Fetch PR details
   - `list_commits` - Get commit messages
   - `get_file_contents` - Analyze changed files
4. Analyze commit messages and file changes
5. Generate concise title (<72 chars) in specified language
6. Preserve existing ticket IDs (e.g., SYN-XXXX)
7. Update PR title using GitHub MCP `update_pull_request` tool

**Dependencies:** Requires GitHub MCP server configured in .mcp.json

### Step 6: Create update-pr-desc Command

**File:** `commands/update-pr-desc.md`

**Content:** Use specs from `specs/2026/init-dev-team-official-claude-plugin/update-pr-desc.md`

**Implementation approach:**
1. Parse arguments: `--pr <number>`, `--lang <ko|eng>`, `--include-load-test`
2. Detect PR number from current branch if not provided
3. Use GitHub MCP server to analyze changes:
   - `get_pull_request` - Get PR metadata
   - `list_commits` - Get all commits in PR
   - `get_file_contents` - Read changed files
   - Use git diff to identify specific changes
   - Identify ViewSet/API changes
   - Detect model/migration changes
   - Find test additions
   - Check for Celery tasks, permissions, security changes
4. **Generate required Mermaid chart(s):**
   - API changes → endpoint flow diagram
   - Model changes → ER diagram
   - Complex logic → process flowchart
   - Multi-component → sequence diagram
   - Apply proper styling with semantic colors
5. Auto-detect PR type (Bug Fix, Feature, Refactoring, etc.)
6. Generate all required sections:
   - PR 타입 (PR Type)
   - 변경 사항 개요 (Change Overview)
   - **변경 사항 시각화 (Change Visualization) - MANDATORY**
   - 관련 이슈 (Related Issues)
   - 테스트 (Tests)
   - Breaking Changes
7. Generate optional sections only if applicable:
   - API 변경 사항
   - 성능 영향
   - 배포 시 주의사항
   - 리뷰어 체크리스트
8. If `--include-load-test`, add results to 성능 영향 section
9. Update PR body using GitHub MCP `update_pull_request` tool

**Critical Requirements:**
- MUST include at least one Mermaid chart
- MUST use color palette for light/dark mode compatibility
- MUST follow template structure from `.github/pull_request_template.md`

**Dependencies:** Requires GitHub MCP server configured in .mcp.json

### Step 7: Create update-docs Command

**File:** `commands/update-docs.md`

**Purpose:** Allow manual triggering of documentation updates

**Implementation:**
1. Scan repository for documentation files (README.md, docs/, *.md)
2. Analyze recent commits/changes
3. Identify documentation that needs updates
4. Generate Mermaid charts for architecture/flow changes
5. Update affected documentation files
6. Report what was updated

### Step 8: Update .mcp.json for GitHub Integration

**File:** `.mcp.json`

Configure GitHub MCP server for PR commands:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Configuration Notes:**
- Users must set `GITHUB_TOKEN` environment variable with a GitHub Personal Access Token
- Token requires permissions: `repo` (full repository access) and `pull_requests` (read/write PR access)
- The MCP server will be auto-started by Claude Code when the plugin is loaded
- Available MCP tools: `get_pull_request`, `update_pull_request`, `list_commits`, `get_file_contents`, etc.

### Step 9: Update README.md

**File:** `README.md`

Add comprehensive documentation:
- Plugin purpose and target projects (synapse-backend, synapse-workspace, synapse-annotator)
- **Installation instructions:**
  - Via Claude Code plugin marketplace (primary method)
  - Manual installation as fallback
- **Prerequisites:**
  - GitHub Personal Access Token setup
  - Required token permissions
- Skills overview:
  - TDD skill - when it activates and how it guides development
  - docs-manager skill - proactive documentation monitoring
  - mermaid-expert skill - chart generation helper
- Commands reference with examples:
  - `/update-pr-title` with all options
  - `/update-pr-desc` with all options
  - `/update-docs` usage
- Configuration requirements (GitHub MCP, environment variables)
- Usage examples for each command with screenshots/examples
- Mermaid chart color conventions reference
- Development and contribution guidelines
- Troubleshooting common issues

### Step 10: Update .gitignore

**File:** `.gitignore`

**Action Required:** REMOVE the following lines from .gitignore:
- `skills/` - Skills must be tracked for distribution
- `commands/` - Commands must be tracked for distribution

**Keep in .gitignore:**
- `agents/` - Keep excluded
- `specs/*` - Already added, keep excluded
- Standard development files (node_modules, .env, logs, etc.)

**Rationale:** Since this plugin will be distributed via Claude Code marketplace, all plugin content (skills and commands) must be version-controlled in the repository.

### Step 11: Add Plugin Metadata for Marketplace

**File:** `.claude-plugin/plugin.json`

Add additional marketplace metadata:

```json
{
  "name": "platform-dev-team-claude-plugin",
  "displayName": "Platform Dev Team",
  "version": "0.1.0",
  "description": "Official Claude plugin for Platform Development Team - TDD guidance, documentation management, and PR management with Mermaid charts",
  "author": {
    "name": "Platform Dev Team"
  },
  "homepage": "https://github.com/your-org/platform-dev-team-claude-plugin",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-org/platform-dev-team-claude-plugin.git"
  },
  "license": "MIT",
  "keywords": ["tdd", "documentation", "pr-management", "mermaid", "dev-team", "django", "backend"],
  "icon": "🛠️",
  "commands": [
    "./commands/update-pr-title.md",
    "./commands/update-pr-desc.md",
    "./commands/update-docs.md"
  ],
  "skills": "./skills/"
}
```

## Detailed File Content Specifications

### TDD Workflow File Structure
```
skills/tdd-workflow/
└── SKILL.md (frontmatter + content from specs lines 28-104)
```

### docs-manager Skill File Structure
```
skills/docs-manager/
├── SKILL.md (main skill definition)
└── mermaid-guidelines.md (reference from specs mermaid-chart-convention.md)
```

### mermaid-expert Skill File Structure
```
skills/mermaid-expert/
└── SKILL.md (specialized Mermaid generation skill)
```

### Command Files
```
commands/
├── update-pr-title.md (from specs)
├── update-pr-desc.md (from specs with GitHub MCP integration)
└── update-docs.md (new - manual doc update trigger)
```

## Critical Files to Create/Modify

1. `.claude-plugin/plugin.json` - Update metadata
2. `skills/tdd-workflow/SKILL.md` - New TDD workflow
3. `skills/docs-manager/SKILL.md` - New docs management skill
4. `skills/docs-manager/mermaid-guidelines.md` - Supporting reference
5. `skills/mermaid-expert/SKILL.md` - New Mermaid helper skill
6. `commands/update-pr-title.md` - New command
7. `commands/update-pr-desc.md` - New command
8. `commands/update-docs.md` - New command
9. `.mcp.json` - Update for GitHub integration
10. `README.md` - Update documentation
11. `.gitignore` - Remove skills/commands exclusions

## Dependencies & Requirements

### External Tools Required
- Git (for TDD skill commit operations and branch detection)
- Node.js/npm (for GitHub MCP server via npx)

### MCP Servers
- **GitHub MCP Server:** `@modelcontextprotocol/server-github`
  - Auto-installed via npx when plugin loads
  - Provides PR read/write capabilities
  - Used by update-pr-title and update-pr-desc commands

### Environment Variables
- `GITHUB_TOKEN` - GitHub Personal Access Token for GitHub MCP server
  - Required permissions: `repo`, `pull_requests`
  - Must be set in user's environment

### Target Projects
- synapse-backend
- synapse-workspace
- synapse-annotator

### Distribution
- Claude Code plugin marketplace (primary method)

## Verification Plan

### 1. Plugin Structure Validation
- Verify all directories exist: `.claude-plugin/`, `skills/`, `commands/`
- Verify plugin.json is valid JSON
- Verify all referenced files exist

### 2. Skill Testing
**TDD Skill:**
- Trigger skill in a test project
- Verify it guides through Red → Green → Refactor cycle
- Verify it enforces commit discipline
- Test Tidy First structural vs behavioral change guidance

**docs-manager Skill:**
- Make a code change in a test project
- Verify skill identifies documentation to update
- Verify Mermaid charts are generated with proper colors
- Test light/dark mode compatibility of charts

**mermaid-expert Skill:**
- Request various diagram types
- Verify proper color styling
- Verify no black/white colors used

### 3. Command Testing
**update-pr-title:**
- Test with `--pr <number>` argument
- Test with current branch auto-detection
- Test with `--lang ko` and `--lang eng`
- Verify ticket IDs are preserved
- Verify title length is appropriate

**update-pr-desc:**
- Test with minimal changes (should generate basic PR desc)
- Test with API changes (should generate endpoint flow diagram)
- Test with model changes (should generate ER diagram)
- Test `--include-load-test` flag
- Verify all required sections are present
- Verify optional sections only appear when relevant
- Verify Mermaid chart colors work in light/dark mode
- Test both Korean and English language options

**update-docs:**
- Make code changes
- Run command
- Verify appropriate documentation is updated
- Verify Mermaid charts are generated

### 4. Integration Testing
- Install plugin in synapse-backend test branch
- Run through complete workflow:
  1. Start feature implementation with TDD skill active
  2. Make changes following TDD cycle
  3. Update documentation with update-docs
  4. Create PR and use update-pr-title and update-pr-desc
  5. Verify all components work together

### 5. GitHub MCP Integration Testing
- Verify GitHub MCP server starts correctly (check Claude Code logs)
- Test MCP tools are accessible:
  - `get_pull_request` - Read PR data
  - `update_pull_request` - Update PR title/body
  - `list_commits` - Get commit history
  - `get_file_contents` - Read file contents
- Verify GitHub token permissions are sufficient
- Test error handling when token is missing/invalid

## Potential Challenges

1. **GitHub MCP Server Setup** - Users must have Node.js/npm installed for npx
2. **Token Configuration** - GitHub token must be properly set in environment with correct permissions
3. **MCP Server Availability** - If MCP server fails to start, commands will not work
4. **Mermaid Rendering** - Charts must be tested in actual GitHub PR view for light/dark mode
5. **Language Handling** - Korean/English switching must work correctly in all sections
6. **Plugin Marketplace Publishing** - Need to follow Claude Code marketplace submission guidelines
7. **Proactive Skill Triggering** - docs-manager must detect changes without being too intrusive

## Success Criteria

- [ ] All skills load without errors
- [ ] All commands appear in slash command menu
- [ ] TDD skill successfully guides development workflow
- [ ] docs-manager correctly identifies and updates documentation
- [ ] update-pr-title generates appropriate titles in both languages
- [ ] update-pr-desc generates complete PR descriptions with Mermaid charts
- [ ] All Mermaid charts render correctly in light and dark modes
- [ ] Plugin can be installed and used in target projects (synapse-backend, etc.)
- [ ] README provides clear usage instructions
