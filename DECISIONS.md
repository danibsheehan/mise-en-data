# Decisions log

## Decided

- **Scope:** 3 movements x 4 films, plus a 4-film baseline group (16 films).
- **Movements:** Soviet Montage, German Expressionism, French Impressionism.
- **Metrics:** average shot length, shot-length distribution, luminance contrast. Motion is an optional stretch goal. Color palette is dropped (the films are black and white or tinted).
- **Pipeline:** Python (PySceneDetect, OpenCV), run offline, outputs JSON.
- **Backend:** Go 1.22+, standard library `net/http`, SQLite via `modernc.org/sqlite` (pure Go, no CGO).
- **Frontend:** React + TypeScript + Vite, D3 for charts.
- **Deploy:** one Docker image with the built frontend embedded in the Go binary, on Fly.io. Fly app names are globally unique, so a variant such as `mise-en-data-app` may be needed.
- **Repo structure:** `client/` (React), `server/` (Go module), `pipeline/` (Python).
- **Go module path:** `github.com/danibsheehan/mise-en-data/server`
- **Repo:** public, MIT license.
- **Theory texts:** linked, never hosted.
- **Out of scope:** the blog write-up, any LLM feature, user accounts, movements beyond the three.

## Pending (author to finalize)

- Claim wording and source texts (steps 1-2).
- Final 16-film list (step 6).

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
