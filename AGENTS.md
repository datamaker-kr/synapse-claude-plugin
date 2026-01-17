# Repository Guidelines

## Project Structure & Module Organization
- `.claude-plugin/plugin.json` defines plugin metadata used by Claude Code.
- `commands/` contains slash-command specs (`*.md`) with YAML frontmatter and step-by-step workflows.
- `skills/` holds skill modules; each `skills/<skill>/SKILL.md` may link to `skills/<skill>/references/`.
- `agents/` provides autonomous agent playbooks invoked by the plugin.
- `docs/claude-code-docs/` stores supporting documentation used for onboarding and reference.

## Build, Test, and Development Commands
- `claude --plugin-dir .` runs this plugin locally in Claude Code for manual verification.
- `/synapse-plugin:help` prints the available commands and requirements.
- `/synapse-plugin:create --name <name> --code <code> --category <type>` scaffolds a new Synapse plugin.
- `/synapse-plugin:test <action> --params '{"key": "value"}'` runs a plugin action locally.
- `/synapse-plugin:update-config` syncs action metadata into config.yaml.
- `/synapse-plugin:dry-run` validates `config.yaml`, entrypoints, and dependencies before publish.
- `/synapse-plugin:publish` publishes the plugin (requires `synapse login`).
- Install prerequisites as needed: `uv pip install synapse-sdk` (or `pip install synapse-sdk`).

## Coding Style & Naming Conventions
- Markdown is the primary format; keep YAML frontmatter at the top of command files.
- Use kebab-case for plugin codes and folder names (e.g., `my-plugin`).
- Keep instructions direct and deterministic; prefer numbered steps and fenced code blocks for commands.
- Use 2-space indentation in YAML examples and align tables for readability.

## Testing Guidelines
- There is no automated test suite in this repo.
- Validate changes by running the relevant slash command in Claude Code and verifying outputs.
- For release readiness, run `/synapse-plugin:dry-run` and at least one `/synapse-plugin:test`.

## Commit & Pull Request Guidelines
- No Git history is available here; use short, imperative messages (e.g., `docs: update dry-run steps`).
- PRs should include: a brief summary, related issue link (if any), and commands run.
- If you change command behavior, update `README.md` and the corresponding file in `commands/`.

## Security & Configuration Tips
- Do not commit secrets; reference env vars in examples (`SYNAPSE_TOKEN`) instead.
- Keep `plugin.json` metadata accurate and avoid introducing local paths or credentials.
