# Project Context Verification

> **Purpose:** Project Lead checklist run BEFORE Phase 1 dispatches. Audits every external reference in `project-context.md` to catch misnamed paths, missing assets, and unverifiable claims. Surfaces problems while they are cheap (a 30-second director conversation) instead of expensive (a Phase 3 agent producing the wrong design).

The Project Lead runs this checklist once after the director hands over `project-context.md`. Every box must be checked or escalated before any specialist agent is dispatched.

---

## Verification checklist

Mark each item ✅ pass, ❌ fail (with brief reason), or `n/a` if the field is not applicable to this project_type.

### Required fields

- [ ] `project_type:` is present and is one of: `internal-tool`, `public-saas`, `open-source-library`, `enterprise-on-prem`
- [ ] `Project Name` is filled in (not placeholder text)
- [ ] `One-Line Description` exists
- [ ] `Tech Stack` section enumerates frontend / backend / database concretely

### External references

For every directory, repository, URL, or file path mentioned in `project-context.md`, verify it exists and is what the director thinks it is.

- [ ] **Theme / brand source paths** — if a directory like `<repo>/<project>` is named as the styling reference, confirm it exists AND that its CSS / design tokens are what the director expects (read 1–2 files, eyeball the palette)
- [ ] **Existing service URLs** — every URL in the "Existing Services" table resolves OR is documented as TBD
- [ ] **Competitor URLs** — homepage and pricing page reachable
- [ ] **Repository paths** — every `<owner>/<repo>` GitHub reference exists and is accessible to the director (org-level apps, etc.)
- [ ] **Local clone paths** — every `<absolute-path>` named as a "work here" location exists locally and matches the named purpose
- [ ] **Asset references** — logos, fonts, illustrations referenced by path actually exist at that path

### Tech stack sanity

- [ ] Every framework / library named is current (not deprecated; major version stated explicitly)
- [ ] Stated versions are consistent (e.g., `Node.js 20` is named once and not contradicted later as `Node.js 18` in another section)
- [ ] Database schema name is named explicitly (not just "Postgres")
- [ ] Auth flow is described concretely enough that the Solutions Architect knows whether to design for JWT / OAuth2 Proxy headers / sessions / something else

### Conventions

- [ ] Test framework is named (not "TBD")
- [ ] Coverage thresholds are stated as numbers, not "high"
- [ ] Conventional Commits / lint / formatter requirements are explicit

### Scope clarity

- [ ] MVP must-haves listed
- [ ] Out-of-scope items listed (NOT implied)
- [ ] Migration requirements stated even if "not applicable"

### Decisions placeholder

- [ ] If the director made decisions during context authoring, they are captured at `docs/decisions/D-NNN-<slug>.md` (not just inline in `project-context.md`)

### Required-tools sanity

- [ ] `.claude/settings.local.json` (or equivalent) allowlist is reviewed against the SKILLs you plan to dispatch in Phase 1. The Project Lead's preflight permission audit will catch missing tools at dispatch time, but checking now avoids the round-trip.

---

## On any ❌

Stop. Do not dispatch Phase 1 agents.

Send the director a single message listing every ❌ with one-sentence explanations + a request for clarification or correction. The director updates `project-context.md`, the Project Lead re-runs this checklist, dispatch resumes when the checklist is clean.

---

## Why this exists

In real runs of the crew, three classes of errors have surfaced AFTER Phase 1 dispatch that this checklist would have caught:

1. **Wrong-path references** — `project-context.md` named a directory the director didn't actually mean. Specialist agents adopted the wrong source of truth and produced output that had to be regenerated.
2. **Missing brand assets** — logos / illustrations referenced by path didn't exist; downstream agents inserted `[PLACEHOLDER]` tags that survived to a phase gate before the gap was noticed.
3. **Implicit scope** — out-of-scope items were assumed not stated; specialist agents rebuilt them anyway because nothing forbade it. Late-cycle director corrections (D-3, D-4, D-11, D-12, etc. patterns) cost dispatches.

A 5-minute pre-dispatch audit is cheap. A Phase 2 redo is not.
