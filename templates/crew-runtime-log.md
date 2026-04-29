# Crew Runtime Log

> **Purpose:** append-only ledger of every dispatch the Project Lead initiates. Captures runtime, token usage (where measurable), and output size so adopters can budget future projects, compare crew configurations across projects, and spot pathological agents.

The Project Lead appends to this file at the end of every dispatch — successful, blocked, or retried. Lives at `<project-root>/docs/crew-runtime-log.md`.

---

## Format

The file is a markdown table with one row per dispatch attempt.

```markdown
# Crew Runtime Log

| # | Date | Phase | Agent | Status | Duration | Tokens (in / out) | Output | Notes |
|---|------|-------|-------|--------|----------|-------------------|--------|-------|
| 1 | 2026-04-20 09:32 | 1 | Market Researcher | Complete | 12m | 8.4k / 41k | docs/research/competitor-analysis.md (41KB) | clean |
| 2 | 2026-04-20 09:32 | 1 | UX Auditor | Complete | 9m | 7.1k / 53k | docs/research/ux-audit.md (53KB) | clean |
| 3 | 2026-04-20 14:18 | 3a | UI/UX Designer | Complete | 18m | 22k / 30k | docs/design/design-system.md (30KB) | superseded by row 5 |
| 4 | 2026-04-20 16:02 | 3b | DBA | Complete | 22m | 19k / 28k | backend/prisma/schema.prisma + migrations | sandbox blocked prisma CLI; hand-wrote SQL |
| 5 | 2026-04-21 08:11 | 3a | UI/UX Designer (regen) | Complete | 14m | 18k / 28k | docs/design/design-system.md (regenerated) | corrected theme source per D-31 |
```

### Column semantics

- **#** — sequential dispatch counter for this project. Never reused, never reordered.
- **Date** — ISO 8601 dispatch timestamp (UTC). Use the moment of dispatch, not completion.
- **Phase** — the phase number; sub-phases (`3a`, `3b`, `3c`) match the SKILL ordering.
- **Agent** — agent role name. Add `(retry)`, `(regen)`, `(scope-N)`, etc. parentheticals when the same role is dispatched more than once.
- **Status** — one of: Complete, Complete (with caveat), Blocked, Failed, Stalled, Cancelled.
- **Duration** — wall clock from dispatch to report-back. Round to the nearest minute.
- **Tokens (in / out)** — input / output token estimates if your platform exposes them; otherwise leave blank.
- **Output** — the deliverable path(s) and aggregate size in KB / LOC. Multiple deliverables comma-separated.
- **Notes** — one-line comment. "clean" is fine. Call out caveats or follow-ups in <80 chars.

---

## How the Project Lead uses this

After every dispatch report-back, the Project Lead appends one row. Do not delete or rewrite rows — when a dispatch is superseded, the supersession is captured in the *new* row's Notes column and (if relevant) a decision in `docs/decisions/`.

End-of-project, the log feeds the **Ship Report's "Crew Runtime" section** (Tech Writer in Phase 6 reads the log and produces a summary table — total dispatches, total wall clock, agent-by-agent breakdown).

---

## Why this exists

Three motivations:

1. **Budget calibration.** When an adopter starts their second project with the crew, they have empirical data from the first to predict cost. Without the log, every project re-runs blind.
2. **Pathological-agent detection.** A specialist that consistently runs over its budget or fails verification at high rates is a signal — either the SKILL needs sharpening or the dispatch prompts are insufficient.
3. **Retro evidence.** When the feedback retrospective asks "where did the crew burn the most time?", the log is the answer instead of memory.

The log is not for performance optimization of individual dispatches in real time — it is a corpus that becomes useful at the 10+ dispatch scale.
