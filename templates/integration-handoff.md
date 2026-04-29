# Integration Handoff

> **Purpose:** name an exact contract between two agents whose work meets at a boundary. Per-pair instance of the artifact pattern — the Solutions Architect's `phase-3-mount-plan.md` lists *which* boundaries exist; this template documents *what* each boundary's contract is.

The producing agent (whoever owns the file or interface being exposed) creates a copy of this template at a sensible path inside their owned tree, fills it out, and commits it as part of their deliverables. The consuming agent reads it before integrating.

Suggested locations:
- `<owned-tree>/INTEGRATION_HANDOFF.md` (e.g., `backend/src/integrations/INTEGRATION_HANDOFF.md`)
- One handoff per major boundary; multiple handoffs may exist in different subtrees.

---

## Integration Handoff — `<short title>`

**Producer:** `<agent name>` (e.g., Integration Engineer)
**Consumer:** `<agent name>` (e.g., Backend Dev)
**Boundary:** `<one-line description of what crosses this boundary — a router export, a typed client, a webhook contract, etc.>`

### What the producer ships

- **Module / file path:** `<absolute path, owned by the producer>`
- **Exported symbols:** `<function names, types, default exports, etc.>`
- **Public signature:** code block showing the exact import the consumer will write
  ```ts
  // example
  import { createGithubRouter } from './integrations/github/routes.js';
  ```
- **Initialization parameters:** `<what the consumer must pass — e.g., a Prisma client, a logger, an env-vars object>`

### How the consumer integrates

- **Mount point or call site:** `<exact file + line where the consumer wires this in>`
- **Snippet:** code block showing the exact mount or invocation
  ```ts
  // example
  api.use('/integrations/github', createGithubRouter(prisma));
  ```
- **Position constraints:** `<must run BEFORE / AFTER which middleware? which routes?>`
- **Side-effect order:** `<what side effects the consumer must NOT trigger before the producer's code runs>`

### Constraints the consumer must NOT violate

- `<list every constraint that, if broken, makes the integration silently incorrect — e.g., "do not apply express.json() globally; this router uses raw bodies for HMAC verification per D-21">`
- `<list any global state the consumer must not touch — e.g., "do not mutate the producer's exported singleton config; clone before extending">`

### Verification

- **Producer self-test:** `<command the producer ran to confirm their export works in isolation — e.g., a unit test path, a curl against a mock server>`
- **Consumer integration test:** `<what test the consumer must add to confirm the boundary is wired correctly>`
- **End-to-end check:** `<a single curl, browser action, or CLI invocation that proves both sides are talking>`

### Open questions

- `<anything the producer is uncertain about — e.g., "does the consumer need access to the raw installation token or just the access token?">`

### Versioning

- **Contract version:** `<v0 / v1 / etc. — bump when a breaking change ships>`
- **Last updated:** `<date>`
- **Last updated by:** `<agent role>`

---

## Why this template exists

When two agents work in parallel on the same codebase, they sometimes converge on incompatible API shapes (URL paths, query param names, payload formats). Writing the contract down BEFORE integration time forces both sides to read the same spec. The producer is responsible for keeping this artifact in sync with their code; the consumer treats it as authoritative.

If the contract changes mid-flight, both sides update this file in the same commit (or PR).
