# tmai-api-spec

Public API contracts for [tmai](https://github.com/trust-delta/tmai) — the UI contract between `tmai-core` and its UI clients (`tmai-ratatui`, `tmai-react`, and any third-party UI).

## What lives here

| File | Purpose |
|------|---------|
| `openapi.json` | OpenAPI 3.1 document describing the HTTP REST API served by `tmai-core` |
| `docs/index.html` | Redoc-based documentation viewer (published via GitHub Pages) |

Future additions (planned):

- `corevents.schema.json` — JSON Schema for SSE `CoreEvent` variants
- `mcp-tools.json` — snapshot of MCP `tools/list` output from `tmai-core`
- Versioned directories (`v0.1.0/`, `v0.2.0/`, ...) for historical specs

## Versioning

This repository follows [SemVer](https://semver.org/) on the API spec itself, independently of `tmai-core` internal versioning.

- **v0.x**: breaking changes are permitted — the API surface is still stabilizing
- **v1.0**: stability declaration, after the following milestones land in `tmai-core`:
  - Orchestrator v2 (policy + LLM) stable under dogfooding
  - AutoActionExecutor MVP (3 scenarios) established
  - Producer Layer minimum implementation shipped

The served HTTP API uses a URL prefix (`/api/v1/`) to version breaking changes.

## Forward compatibility rules for UI clients

When consuming this spec from a UI client (`tmai-ratatui`, `tmai-react`, or a third-party UI):

- **Unknown CoreEvent variants MUST be ignored** — new event variants will be added as non-breaking changes.
- **Unknown optional fields SHOULD be tolerated** — new optional fields will be added without a version bump.
- **Required field changes, type changes, endpoint removals** trigger a major version bump.

## Building a UI on this spec

`tmai-core` exposes the following contract surfaces:

1. **HTTP REST** (this repository's `openapi.json`)
2. **SSE event stream** at `/api/events` — forthcoming JSON Schema documents each `CoreEvent` variant
3. **MCP** (stdio JSON-RPC 2.0) — `tools/list` snapshot forthcoming

UI clients can choose any subset of these channels. The standard UIs `tmai-ratatui` (Rust TUI) and `tmai-react` (React WebUI) both use HTTP + SSE; MCP is primarily consumed by Claude Code.

## Documentation

Hosted at https://trust-delta.github.io/tmai-api-spec/ (Redoc-rendered).

## Status

**v0.0.1 — bootstrap.** Contains the initial PoC endpoint (`GET /api/task-meta`) migrated from `tmai-core` issue #446. The remaining ~79 endpoints will be added incrementally; breaking changes are permitted at any time during v0.x.

## License

MIT — see [LICENSE](LICENSE).
