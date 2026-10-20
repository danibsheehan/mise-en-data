# Decisions log

## Decided

- **Scope:** 3 movements x 4 films, plus a 4-film baseline group (16 films).
- **Movements:** Soviet Montage, German Expressionism, French Impressionism.
- **Metrics:** average shot length, shot-length distribution, luminance contrast. Motion is an optional stretch goal. Color palette is dropped (the films are black and white or tinted).
- **Pipeline:** Python (PySceneDetect, OpenCV), run offline, outputs JSON.
- **Backend:** Go 1.22+, standard library `net/http`, no database. The pipeline output and curated content are JSON embedded in the binary with `go:embed`, loaded and validated once at startup (Option A).
- **Frontend:** React + TypeScript + Vite, D3 for charts.
- **API contract:** hand-written TypeScript types in `client/src/api/types.ts`, checked by a Go golden-JSON test. OpenAPI is a later follow-up.
- **Hosting:** frontend on GitHub Pages at the default `github.io/mise-en-data/` path; API on Cloud Run in a new GCP project. Reusable workflows from `dani-actions` where they exist, plus a small in-repo Python CI job for the pipeline.
- **Data location:** pipeline output and the author's curated content live as JSON in `server/internal/dataset/data/`. Pipeline files are generated and never hand-edited; curated files are the author's.
- **Dependabot:** `main` already covers GitHub Actions. Entries for gomod (`/server`), npm (`/client`) and pip (`/pipeline`) are added in the chunk where each ecosystem first lands.
- **Repo structure:** `client/` (React), `server/` (Go module), `pipeline/` (Python).
- **Go module path:** `github.com/danibsheehan/mise-en-data/server`
- **Repo:** public, MIT license.
- **Theory texts:** linked, never hosted.
- **Out of scope:** the blog write-up, any LLM feature, user accounts, movements beyond the three.

## Pending (author to finalize)

- Claim wording and source texts (steps 1-2).
- Final 16-film list (step 6).

## Revisit after first deploy

- **A database.** Options: a read-only SQLite file built into the image, or a new Supabase project (the last free slot; musing uses the other). Not needed now: the data is a few hundred KB, read-only and regenerated offline.

## Rationale (2026-10-20)

- **No database:** nothing requires one. A database would add failure modes (for example Supabase pausing free projects after a week of inactivity) and no capability.
- **Pages + Cloud Run:** keeps cost close to $0 and matches the setup in musing and caught-looking, so maintenance is familiar.
- **Superseded:** the original handoff choices of SQLite via `modernc.org/sqlite` and a single Docker image on Fly.io, with the frontend embedded in the Go binary.

## Rights and data-source terms

Not yet checked. Log findings here (steps 4-5).

### Public-domain status per title

| Film | Movement | Chosen upload | US status | Notes |
|------|----------|---------------|-----------|-------|

### Data sources

| Source | Terms checked | Findings |
|--------|---------------|----------|
| Internet Archive | no | |
| TMDB (if used) | no | |
| Wikidata (if used) | no | |
