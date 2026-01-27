# Update PR Description Command

You will help the user update a PR description following the enhanced backend API server template.

## Available Options:

- `--pr <number>`: Specify PR number (optional - uses current branch if not provided)
- `--lang <language>`: Description language (korean/english, default: korean)
- `--include-load-test`: Include contents about 'Load Test' result into PR description

## Examples:

- `/update-pr-desc --pr 123 --lang ko`: Update PR #123 with Korean description
- `/update-pr-desc --lang eng`: Update current branch PR with English description
- `/update-pr-desc --pr 123 --lang ko --include-load-test`: Update PR #123 with Korean description and load test result
- `/update-pr-desc`: Update current branch PR with Korean description (default)

## Template Compliance

The PR description MUST follow the structure defined in `.github/pull_request_template.md`:

### Required Sections (필수)

1. **PR 타입 (PR Type)** 🏷️
   - Check appropriate type: Bug Fix, Feature, Refactoring, Documentation, Performance, Test, Configuration, Deployment, Security

2. **변경 사항 개요 (Change Overview)** 🛠
   - 주요 변경 내용 (Main changes): Summarize what was changed
   - 변경 이유 (Reason for change): Explain why this change was needed

3. **변경 사항 시각화 (Change Visualization)** 📊 **[MANDATORY]**
   - **Must include at least one Mermaid chart** visualizing the changes
   - Choose appropriate diagram type(s):
     - **API 엔드포인트 변경**: Flow diagram showing Client → API → ViewSet → Serializer → Service → DB
     - **데이터베이스 스키마 변경**: ER diagram showing model relationships
     - **처리 흐름도**: Flowchart showing business logic flow with decision points
     - **시퀀스 다이어그램**: Sequence diagram for complex multi-component interactions
   - **Use standardized colors** (light/dark mode compatible):
     - Blue (#3b82f6): Normal operations, default states
     - Green (#10b981): Success, completion
     - Yellow (#f59e0b): Warnings, pending
     - Red (#ef4444): Errors, failures
     - Purple (#8b5cf6): Special states
     - Cyan (#06b6d4): Info items
   - Include `style` directives for all nodes

4. **관련 이슈 (Related Issues)** 📝
   - Link to JIRA ticket or GitHub issue (e.g., `Closes SYN-1234`, `Fixes #456`)

5. **테스트 (Tests)** ✅
   - Test checklist: Django tests, API tests, permissions, models, serializers, migrations, Celery tasks, tenant isolation
   - Test coverage: 80%+ for new code, 100% for critical paths
   - Test execution commands: `python manage.py test`
   - Manual test scenarios

6. **Breaking Changes** ⚠️
   - Indicate if there are breaking changes
   - Provide detailed explanation if applicable

### Optional Sections (선택)

Only include if relevant; delete section if not applicable:

7. **API 변경 사항 (API Changes)** 🔌
   - Table of new/modified endpoints (Method, Endpoint, Description, Change Type)
   - Request/Response examples in collapsible details
   - OpenAPI schema update checklist

8. **성능 영향 (Performance Impact)** ⚡
   - Database performance (query optimization, N+1 issues, indexing)
   - API response performance (response time, pagination, caching)
   - Resource usage (memory, CPU, concurrency)
   - Performance measurement results (Before/After metrics)

9. **배포 시 주의사항 (Deployment Notes)** 🚀
   - Database migrations (with rollback plan)
   - Environment configuration (.env, settings, dependencies)
   - Cache and data (Redis, static files)
   - Service restart sequence (Django, Celery, Nginx)
   - Deployment order

10. **리뷰어 체크리스트 (Reviewer Checklist)** 👀
    - Code quality (TDD, Ruff, DRF patterns, DRY)
    - Architecture (model design, business logic placement)
    - Security (auth/permissions, SQL injection, XSS, CSRF)
    - Performance (N+1 queries, indexing, caching, async)
    - Tests (APITestCase, coverage, edge cases, permissions)
    - Database (migrations, constraints, pgtrigger, pghistory)

11. **기타 참고사항 (Additional Notes)** 💡

## Implementation Steps:

1. **Detect PR Number**
   - If `--pr` not provided, find PR for current branch using GitHub API
   - If no PR found, show error message

2. **Analyze Changes**
   - Get commit diff and file changes
   - Identify changed files (models, views, serializers, tests, migrations)
   - Detect API endpoint changes (ViewSets, APIViews)
   - Identify database schema changes (models, migrations)
   - Detect test additions/modifications
   - Check for Celery tasks, permissions, security changes

3. **Generate Mermaid Charts** (Required)
   - **Always generate at least one chart** based on changes detected:
     - If ViewSet/API changes: Generate API endpoint flow diagram
     - If model changes: Generate ER diagram
     - If complex logic changes: Generate process flowchart
     - If multi-component interaction: Generate sequence diagram
   - Apply proper styling with color codes for light/dark mode compatibility
   - Use semantic colors (green=success, red=error, yellow=warning, blue=normal)

4. **Determine PR Type**
   - Auto-detect based on files changed and commit messages:
     - Bug fix: Files in bug fixes, "fix" in commits
     - Feature: New ViewSets, models, "feat" in commits
     - Refactoring: Code restructuring without behavior change
     - Performance: Query optimizations, caching additions
     - Test: Mainly test file changes
     - Security: Authentication, permission, validation changes
     - Configuration: Settings, environment, deployment changes

5. **Generate Sections**
   - **변경 사항 개요**: Summarize main changes and reasons
   - **변경 사항 시각화**: Generate appropriate Mermaid chart(s)
   - **관련 이슈**: Extract from branch name or ask user
   - **API 변경 사항**: List endpoints if API changes detected
   - **테스트**: Include relevant test checklist items based on changes
   - **성능 영향**: Include if performance-related changes detected
   - **Breaking Changes**: Flag if API signature changes or migrations alter schemas
   - **배포 시 주의사항**: Include if migrations, env vars, or service changes detected
   - **리뷰어 체크리스트**: Suggest relevant items based on change type

6. **Load Test Results** (if `--include-load-test` flag)
   - Add load test results to 성능 영향 section
   - Include before/after metrics (avg, p95, p99)

7. **Language Selection**
   - Korean (default): Use Korean for all section descriptions
   - English: Use English for all section descriptions
   - Code examples and technical terms remain in English

8. **Update PR**
   - Use GitHub API to update PR description
   - Preserve any existing content that should be kept
   - Format properly with markdown

## Critical Requirements:

- **MUST include at least one Mermaid chart** - this is mandatory
- **MUST use proper color styling** for light/dark mode compatibility
- **MUST follow template structure** - required sections cannot be omitted
- **SHOULD delete optional sections** that are not applicable
- **MUST provide concrete examples** in API changes section (if applicable)
- **MUST include test commands** and coverage expectations
- **MUST check for breaking changes** and flag them clearly

## Color Palette Reference:

```
Primary Colors:
- Blue:   #3b82f6  (Primary actions, normal states)
- Green:  #10b981  (Success, completion)
- Yellow: #f59e0b  (Warnings, caution)
- Red:    #ef4444  (Errors, failures)
- Purple: #8b5cf6  (Special states)
- Cyan:   #06b6d4  (Info)

Neutral Colors:
- Light Gray: #e5e7eb
- Medium Gray: #6b7280
- Dark Gray: #374151
```

Please provide the PR number and desired language, or use defaults for current branch and Korean.
