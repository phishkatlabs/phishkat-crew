# Phase 3 Mount Plan

**Authored by:** Solutions Architect (Phase 2)
**Read by:** Every Phase 3 build agent before they start
**Purpose:** Eliminate parallel-dispatch coordination failures by naming file ownership and integration contracts up front.

## File ownership

| Path glob | Owner agent | Notes |
|---|---|---|
| `backend/src/routes/**/*.ts` | Backend Dev | except integrations/ |
| `backend/src/services/**/*.ts` | Backend Dev | |
| `backend/src/middleware/**/*.ts` | Backend Dev | |
| `backend/src/integrations/**/*` | Integration Engineer | |
| `backend/prisma/schema.prisma` | DBA | |
| `frontend/**/*` | Frontend Dev | |
| `cli/**/*` | Integration Engineer | if a CLI exists |
| `Dockerfile`, `docker-compose.yml`, `.github/workflows/*` | DevOps (Phase 6) | |
| Project root docs (README, ARCHITECTURE, API, CHANGELOG) | Tech Writer (Phase 6) | |
| `CONTRIBUTING.md`, `.github/ISSUE_TEMPLATE/*` | Community Manager (Phase 6) | |

## Integration handoff contracts

For every cross-agent boundary, name the artifact:

| Producer | Consumer | Artifact | Format |
|---|---|---|---|
| Integration Engineer | Backend Dev | `backend/src/integrations/INTEGRATION_HANDOFF.md` | Path of router export + exact mount snippet |
| Backend Dev | Frontend Dev | API contracts in system-design.md §3 | Inline — Frontend Dev reads architect's spec; mismatches go in `frontend/FRONTEND_API_NOTES.md` |
| Backend Dev | Frontend Dev | Updated env contract | `backend/.env.example` — Frontend Dev reads to know what's expected |

## Cross-agent collision risks (call out NOW)

[List any case where two agents would naturally edit the same file. State who wins and what the loser does instead. Example: "Both Backend Dev and Integration Engineer want to mount routes. **Backend Dev owns** `backend/src/routes/index.ts`. **Integration Engineer's contract:** export a router factory and document the mount in `INTEGRATION_HANDOFF.md`."]
