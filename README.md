# mise-en-data

A portfolio project that tests classic film-theory claims against shot-level data from silent-era films. Each claim gets a verdict (Supported / Partly supported / Not supported / Inconclusive) backed by measurements.

> Work in progress. Setup, architecture and a decisions-log summary will be added here (Phase 6).

## Structure

- `client/` — React + TypeScript + Vite frontend
- `server/` — Go API
- `pipeline/` — Python fingerprint pipeline (run offline, outputs JSON)

See [DECISIONS.md](DECISIONS.md) for the decisions log.

## License

MIT. See [LICENSE](LICENSE).
