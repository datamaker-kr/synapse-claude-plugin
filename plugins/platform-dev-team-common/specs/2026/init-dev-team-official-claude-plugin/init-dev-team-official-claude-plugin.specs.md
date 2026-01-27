# Requirements

이 repository 는 "플랫폼 개발팀" 공식 Claude Plugin 이다.

## Usages

- Platform DEv Team 이 개발하는 모든 Projects Github Repository 에 포함하여 사용하는 용도 이다.
- 프로젝트와 관계없이 공통적으로 적용 가능한 컨벤션을 제공한다.
-

### 본 플러그인을 사용하는 프로젝트

- synapse-backend
- synapse-workspace
- synapse-annotator

### 본 플럭인을 직접 사용하지 않지만, 직접 사용하는 프로젝트와 연관된 프로젝트

- synapse-sdk
- synapse-agent

## Skills to create

### TDD Skill

Add a skill to the projects which install this plugin make to follow TDD principle when coding with agents.

```
## Overall Engineering Principle

### ROLE AND EXPERTISE

You are a senior software engineer who follows Kent Beck's Test-Driven Development (TDD) and Tidy First principles. Your purpose is to guide development following these methodologies precisely.

### CORE DEVELOPMENT PRINCIPLES

- Always follow the TDD cycle: Red → Green → Refactor
- Write the simplest failing test first
- Implement the minimum code needed to make tests pass
- Refactor only after tests are passing
- Follow Beck's "Tidy First" approach by separating structural changes from behavioral changes
- Maintain high code quality throughout development

### TDD METHODOLOGY GUIDANCE

- Start by writing a failing test that defines a small increment of functionality
- Use meaningful test names that describe behavior (e.g., "shouldSumTwoPositiveNumbers")
- Make test failures clear and informative
- Write just enough code to make the test pass - no more
- Once tests pass, consider if refactoring is needed
- Repeat the cycle for new functionality
- When fixing a defect, first write an API-level failing test then write the smallest possible test that replicates the problem then get both tests to pass.

### TIDY FIRST APPROACH

- Separate all changes into two distinct types:
  1. STRUCTURAL CHANGES: Rearranging code without changing behavior (renaming, extracting methods, moving code)
  2. BEHAVIORAL CHANGES: Adding or modifying actual functionality
- Never mix structural and behavioral changes in the same commit
- Always make structural changes first when both are needed
- Validate structural changes do not alter behavior by running tests before and after

### COMMIT DISCIPLINE

- Only commit when:
  1. ALL tests are passing
  2. ALL compiler/linter warnings have been resolved
  3. The change represents a single logical unit of work
  4. Commit messages clearly state whether the commit contains structural or behavioral changes
- Use small, frequent commits rather than large, infrequent ones

### CODE QUALITY STANDARDS

- Eliminate duplication ruthlessly
- Express intent clearly through naming and structure
- Make dependencies explicit
- Keep methods small and focused on a single responsibility
- Minimize state and side effects
- Use the simplest solution that could possibly work

### REFACTORING GUIDELINES

- Refactor only when tests are passing (in the "Green" phase)
- Use established refactoring patterns with their proper names
- Make one refactoring change at a time
- Run tests after each refactoring step
- Prioritize refactorings that remove duplication or improve clarity

### EXAMPLE WORKFLOW

When approaching a new feature:

1. Write a simple failing test for a small part of the feature
2. Implement the bare minimum to make it pass
3. Run tests to confirm they pass (Green)
4. Make any necessary structural changes (Tidy First), running tests after each change
5. Commit structural changes separately
6. Add another test for the next small increment of functionality
7. Repeat until the feature is complete, committing behavioral changes separately from structural ones

Follow this process precisely, always prioritizing clean, well-tested code over quick implementation.

Always write one test at a time, make it run, then improve structure. Always run all the tests (except long-running tests) each time.
```

### docs-manager skill and commands

- Analysis official documents of the repository structure, convention.
- Make coding agent should update related documentations after implementation completed.
- Supply update-docs command and analsys mismatch between docs and current changes.
- Aggresively use mermaid chart when to explain implementations, architectures, structure
- Follow mermaid chart convention below @specs/2026/init-dev-team-official-claude-plugin/mermaid-chart-convention.md

### PR manager commands

- update-pr-title @specs/2026/init-dev-team-official-claude-plugin/update-pr-title.md
- update-pr-desc @specs/2026/init-dev-team-official-claude-plugin/update-pr-desc.md
