# AGENTS.md

Instructions for any coding agent (Cursor, Claude Code, or otherwise) working in this repo.
Human contributors: see `README.md` instead — this file is written for agents and skips the
narrative tour.

mise-en-data is a portfolio project that tests classic film-theory claims against shot-level
data from silent-era films. Each claim gets a verdict (Supported / Partly supported / Not
supported / Inconclusive) backed by measurements. Scope and decisions are logged in
`DECISIONS.md`.

## Stack

- `client/` — React + TypeScript + Vite frontend, D3 for charts.
- `server/` — Go 1.22+ API using standard library `net/http`, no database: pipeline output
  and curated content are JSON embedded with `go:embed` and loaded at startup. Module path:
  `github.com/danibsheehan/mise-en-data/server`.
- `pipeline/` — Python (PySceneDetect, OpenCV) fingerprint pipeline, run offline, outputs
  JSON.
- Hosting: frontend on GitHub Pages, API on Cloud Run. See `DECISIONS.md`.

## Install

Not set up yet; added as each part of the stack lands.

## Configure

Not set up yet.

## Run

Not set up yet.

## Test / CI parity

Not set up yet. The only CI today is `.github/workflows/pr-title-lint.yml` (PR titles must
follow Conventional Commits). Stack-specific jobs are added as each part of the stack lands.

## Conventions

- Metrics: average shot length, shot-length distribution, luminance contrast. Motion is an
  optional stretch goal. Color palette is dropped (the films are black and white or tinted).
- Scope: 3 movements (Soviet Montage, German Expressionism, French Impressionism) x 4 films,
  plus a 4-film baseline group.
- Log new decisions in `DECISIONS.md`.

## Constraints — do not

- **Commit or push directly to `main`.** Always branch first, following
  `foundations:branch-naming`'s `<type>/<slug>` convention, then open a PR — even for small
  or config-only changes.
- **Host theory texts.** Link to them, never host them.
- **Show frames from restored or scored film versions**, or add a film whose chosen version has
  new or recreated intertitles or colour. See "Rights posture" in `DECISIONS.md`.
- **Add out-of-scope work**: a blog write-up, any LLM feature, user accounts, or movements
  beyond the three.

## Definition of done

- **Task done**: format, lint, test and build for whatever part of the stack you changed,
  once those exist — see `foundations:definition-of-done` for the check sequence and when to
  scale it up or down.
- **PR done**: the CI-parity sequence above plus a why-first PR description, with a
  Conventional Commits title — see **pr-ready**.

---

Skills: this repo installs the shared `foundations` plugin from the `dani-foundations`
marketplace for skills common across personal projects — see `CLAUDE.md`. Three apply to
how any task here gets branched, scoped, and verified: `foundations:branch-naming` (branch
names follow `<type>/<slug>`; enforced automatically by a `PreToolUse` hook shipped with the
plugin), `foundations:pr-chunk-plan` (break a task touching 3+ files or bundling multiple
concerns into small reviewable chunks before implementation starts), and
`foundations:definition-of-done` (run format/lint/test/build after substantive edits and
before committing, scaled to the size of the change).
