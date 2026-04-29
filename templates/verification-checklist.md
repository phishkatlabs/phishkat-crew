# Verification Checklist (Director-Run)

> **Purpose:** every agent that produces work the director must validate locally — DBA migrations, Backend Dev API, Frontend Dev UI, DevOps deploys, Integration Engineer external services — emits a `verification-checklist.md` alongside their normal deliverables. The director runs through this checklist BEFORE the Project Lead advances the gate.

This makes the human-in-the-loop step explicit instead of ad-hoc. Without it, gate criteria like "DBA migrations applied successfully" become assumptions the agent makes about a sandbox that may not match the director's actual machine.

The producing agent fills this template out and saves it at a path under their owned deliverables tree (e.g., `backend/VERIFICATION.md`, `frontend/VERIFICATION.md`, `docs/devops-verification.md`). Multiple checklists may exist per phase.

---

## Verification Checklist — `<deliverable name>`

**Produced by:** `<agent role>`
**Phase:** `<N>`
**Director runs this:** before the Project Lead can advance the next phase gate.

### What you are verifying

`<one paragraph — what was built, why the director needs to confirm it works on their machine specifically (e.g., because the agent's sandbox lacks live tools the user has), and what failure looks like>`

### Prerequisites

Before running the steps below, the director should have:

- [ ] `<each tool / runtime / service the director needs to have available>`
- [ ] `<each env var or secret the director needs to have set>`
- [ ] `<each Docker container or external service the director needs running>`

If any prerequisite is missing, do not run this checklist — escalate to the Project Lead with the missing item.

### Steps

Each step is a single director-runnable command + the expected outcome. Do not skip steps.

1. **`<command>`**
   - Expected: `<exact output line, exit code, or observable side effect>`
   - On failure: `<what to check, what error class points to which root cause>`

2. **`<command>`**
   - Expected: `<...>`
   - On failure: `<...>`

3. *(...)*

### Common failure modes

- **`<failure pattern 1>`** → root cause + fix path
- **`<failure pattern 2>`** → root cause + fix path

### Rollback (if the verification fails partway through)

`<exact commands to revert to a known-good state — e.g., docker compose down + drop schema + re-seed>`

### What to do on success

Reply to the Project Lead with:

```
## Director Verification — <deliverable name>
**Status:** PASS
**Steps run:** N of N
**Notes:** <any anomalies that didn't fail but seemed worth flagging>
```

The Project Lead then advances the gate.

---

## Why every produced-and-verified deliverable needs a checklist

In real crew runs, three patterns repeatedly cost time:

1. **The agent's sandbox lacked a tool the director has** — the agent reported success against a degraded substrate; the director's machine surfaced bugs. Without an explicit director-side verification step, those bugs land in a later phase.
2. **Hidden state drift** — agent-installed migrations, seeded fixtures, or generated env files exist in the sandbox but not on the director's machine; subsequent live work fails until the gap is filled.
3. **Smoke testing was implicit** — the director was expected to "just open the app and look around" without a structured list. Some interactive surfaces were never visited; bugs surfaced only post-ship.

A 5-minute structured checklist eliminates all three classes.
