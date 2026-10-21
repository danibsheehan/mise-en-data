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
- Soviet Montage and German Expressionism titles: rights check not yet done.
- Impressionist and baseline titles are chosen (see "Film list"). A viewing check for added intertitles and recreated colour is outstanding for each chosen upload.
- Which *Usher* upload (see "Film list").

## Revisit after first deploy

- **A database.** Options: a read-only SQLite file built into the image, or a new Supabase project (the last free slot; musing uses the other). Not needed now: the data is a few hundred KB, read-only and regenerated offline.

## Rationale (2026-10-20)

- **No database:** nothing requires one. A database would add failure modes (for example Supabase pausing free projects after a week of inactivity) and no capability.
- **Pages + Cloud Run:** keeps cost close to $0 and matches the setup in musing and caught-looking, so maintenance is familiar.
- **Superseded:** the original handoff choices of SQLite via `modernc.org/sqlite` and a single Docker image on Fly.io, with the frontend embedded in the Go binary.

## Rights posture

Not legal advice; the research below is the author's working record.

- **Public domain by date.** Works first published in 1930 or earlier are in the US public domain as of 2026 ([Duke, Public Domain Day 2026](https://web.law.duke.edu/cspd/publicdomainday/2026/)). Foreign works restored by the URAA get the remainder of a 95-year term, so pre-1930 films are not revived ([Copyright Office Circular 38B](https://www.copyright.gov/circs/circ38b.pdf), read via a search summary). US status only; other countries differ.
- **Versions.** A score in a restored or re-scored version is allowed. New or recreated intertitles and recreated colour are not (viewing check outstanding). No frames from restored or scored versions are shown on the site. Frames are parked for now.
- **Footage.** Downloaded only into the gitignored `pipeline/raw/` on the author's machine. Only derived measurements are published.
- **Fair use is a fallback, not the basis.** It is decided case by case, and only matters for in-copyright material or added rights in a version. Findings:
  - Copying whole works to analyse them was fair use in [*Authors Guild v. HathiTrust*](https://www.copyright.gov/fair-use/summaries/authorsguild-hathitrust-2dcir2014.pdf) (2d Cir. 2014).
  - Low-resolution thumbnails serving a different function were transformative in [*Kelly v. Arriba Soft*](https://cyber.harvard.edu/property00/metatags/ARRIBA.html) and [*Perfect 10 v. Amazon*](https://en.wikipedia.org/wiki/Perfect_10,_Inc._v._Amazon.com,_Inc.) (9th Cir.).
  - [*Warhol v. Goldsmith*](https://www.copyright.gov/fair-use/summaries/Andy-Warhol-Found-for-the-Visual-Arts-Inc-v-Goldsmith-143-S-Ct-1258-2023.pdf) (2023): new expression is not enough; the use needs a different purpose, and commentary must conjure up the original.
  - *Authors Guild v. Google* (2d Cir. 2015) was not verified.
  - A text-and-data-mining exemption under DMCA section 1201 covers motion pictures ([Authors Alliance](https://www.authorsalliance.org/2024/10/25/text-data-mining-research-dmca-exemption-renewed-and-expanded/)). Whether it covers an independent individual is unconfirmed.
- **Measurement notes.** Silent-film runtime and shot length depend on playback speed. Some uploads are 25 fps (PAL) transfers, for example *Broken Blossoms*. Cinemetrics' silent-era figures use the projection speed that gives natural motion. The hand-count validation (handoff step 11) should note the frame rate used.

## Film list

Impressionism and baseline are chosen. Soviet Montage and German Expressionism are not yet checked. Probe values were read over HTTP from each Internet Archive file's stream headers and a 90-second loudness sample, and no footage was saved.

| Film | Group | Director, year | Candidate upload (IA id) | Runtime | Video | Audio | Version notes and open checks |
|------|-------|----------------|--------------------------|---------|-------|-------|-------------------------------|
| *The Smiling Madame Beudet* | Impressionism | Dulac, 1923 | `the-smiling-madame-beudet` | 42.7 min | 992x720 at 23.976 fps | AAC, mean -22.5 dB (score likely) | A second upload with the same runtime is a Blu-ray rip. The EYE Filmmuseum/Haghefilm restoration is likely (inference). Viewing check for titles and colour. |
| *The Fall of the House of Usher* | Impressionism | Epstein, 1928 | Candidates: `the_fall_of_the_house_of_usher_jean_epstein` (WEB-DL rip, 1920x1080 at 24 fps, mean -33.9 dB) and `jean-epsteins-the-fall-of-the-house-of-usher-1928` (1280x720 at 30 fps, mean -34.0 dB) | 65.4 min | | AAC | Neither upload has a PD label. Upload not yet chosen. The colorized upload `bos_20240929` is excluded. |
| *L'Argent* | Impressionism | L'Herbier, 1928 | `l-argent-1928-par-marcel-l-herbier` | 150.7 min | 1440x1072 at 22 fps | AAC, mean -23.4 dB | A Blu-ray rip. The uploader says "PD in USA". Lobster's release is a 4K rescan of the camera negative, and about 150 of roughly 200 original minutes survive. Viewing check for titles and colour. |
| *Fièvre* | Impressionism | Delluc, 1921 | `fievre-louis-delluc-1921-1080p` | 44.6 min | 1920x1080 at 24 fps | AAC, mean -36.1 dB | No description on the item. Source release unknown. Viewing check for titles and colour. |
| *Broken Blossoms* | Baseline | Griffith, 1919 | `Broken.Blossoms` | 88.8 min | 512x384 at 25 fps (PAL) | none | The runtime matches MoMA's 89-minute tinted restoration. Kino's 90-minute edition has a new score and tints. Source unknown. |
| *Sherlock Jr.* | Baseline | Keaton, 1924 | `MyMovie_20190318` | 44.1 min | 640x480 at 29.97 fps | AAC, silent (-91 dB) | Silent Hall of Fame upload. Source unknown. |
| *The Freshman* | Baseline | Lloyd, 1925 | `the-freshman-1925_202401` | 75.4 min | 1440x1080 at 23.976 fps | none | The runtime matches Criterion's edition from the 1998 UCLA restoration (76 min), so probably a disc rip with the audio removed (inference). |
| *The Gold Rush* | Baseline | Chaplin, 1925 | `the-gold-rush-film-1925` | 86.6 min | 960x720 at 24 fps | AAC, silent (-91 dB) | The description says it is the 1925 silent original; the 1942 sound re-release is excluded. |

An audio track is not proof of an added score, and a missing one does not rule out added titles or colour. The Internet Archive rights labels are set by uploaders and are not checks.

## Data sources

| Source | Terms checked | Findings |
|--------|---------------|----------|
| Internet Archive | no | The Terms of Use page could not be read. Item pages show uploader-assigned rights labels only. |
| TMDB (not planned) | yes | [API terms](https://www.themoviedb.org/api-terms-of-use): non-commercial use only, with the TMDB logo and a non-endorsement notice. No derivatives, no caching beyond 6 months. |
| Wikidata (not planned) | yes | Structured data is CC0 ([licensing](https://www.wikidata.org/wiki/Wikidata:Licensing)). |
| Cinemetrics | no | Fallback for shot-length data if a download proves unusable. Crowd-sourced, with no second-step verification. Data-reuse terms are unresolved: its Terms link points to generic OCHRE software terms. |
| PySceneDetect, OpenCV | no | Licences not yet checked. |
