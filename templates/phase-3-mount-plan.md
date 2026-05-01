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

## Dependency arrows (cross-file imports across agent boundaries)

When Agent A's deliverable **imports from** Agent B's deliverable AND both run in parallel, declare the dependency explicitly so the consuming agent doesn't compile-fail on a missing module. State which sanctioned pattern applies:

| Importer (Agent A) | Imported (Agent B) | Sanctioned pattern | Notes |
|---|---|---|---|
| `backend/src/jobs/runFinalizer.ts` (Backend Dev) | `backend/src/integrations/github/prComment.ts` (Integration Engineer) | **Sequenced dispatch** OR **integration-handoff stub pattern** | Pick one; default to sequenced (B before A) when feasible |

### Sanctioned patterns

**A. Sequenced dispatch** — the safer default. The Project Lead dispatches the producer (Agent B) first, waits for Complete, THEN dispatches the consumer (Agent A). Tradeoff: serialization, slightly longer wall clock. Use when the parallel-dispatch speedup isn't worth the risk.

**B. Integration-handoff stub pattern** — when parallel speedup matters. The consumer's brief instructs them to wire a graceful-fallback seam: a dynamic import that no-ops if the target file doesn't exist yet, plus a clear marker in code so the next reader knows this is a deliberate seam (not a forgotten TODO). The producer's brief mentions the consumer is depending on a specific export shape, and they MUST hit that shape exactly. Tradeoff: one extra layer of indirection in the consumer's code; producer must respect the spec literally.

Sample seam shape (TypeScript, dynamic import — adapt for the project's idiom):

```ts
// In Agent A's file, importing Agent B's not-yet-existent module:
async function callPrComment(...args: PrCommentArgs): Promise<void> {
  try {
    const m = await import('../integrations/github/prComment');
    await m.postRunSummaryComment(...args);
  } catch (err: unknown) {
    // Module may not exist yet during parallel dispatch.
    // After Agent B's Complete, this becomes a normal call.
    logger.warn({ err }, '[github] prComment unavailable — skipping (acceptable during phase-3 parallel dispatch)');
  }
}
```

For each dependency arrow you list, choose A or B and document the choice. If you choose B, the producer's brief MUST specify the exact export shape the consumer is coding against — under-specification of B causes a different class of failure (consumer compiles, but at runtime the module exists with a slightly different signature and silently no-ops).
