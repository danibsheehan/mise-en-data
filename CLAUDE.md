# CLAUDE.md

@AGENTS.md

The above is the canonical, tool-agnostic reference (install/configure/run/test, conventions,
constraints, definition of done) — also read by Cursor and any other agent. Everything below
is Claude Code–specific session mechanics.

## Skills

This repo has no repo-specific skills yet. It installs the shared `foundations` plugin from
the `dani-foundations` marketplace (see `.claude/settings.json`) for process/docs skills
common across projects (doc-writer, pr-ready, definition-of-done, dependabot-triage,
coverage-gap-diagnosis, pr-summary-draft, test-generator).
