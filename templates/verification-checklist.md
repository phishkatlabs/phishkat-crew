# Verification Checklist (Project-Lead-Run by default)

> **Purpose:** every agent that produces work that needs validating against the director's actual environment — DBA migrations, Backend Dev API, Frontend Dev UI, DevOps deploys, Integration Engineer external services, Migration Specialist tooling — emits a `verification-checklist.md` alongside their normal deliverables. The Project Lead runs through this checklist BEFORE advancing the phase gate.

The checklist makes the verification step explicit instead of ad-hoc. Without it, gate criteria like "DBA migrations applied successfully" become assumptions the agent makes about a sandbox that may not match the director's actual machine.

## v0.4 — who runs the steps

By default the **Project Lead** executes verification steps directly, captures proof of execution (command output, status codes, query results, etc.), and reports a single consolidated PASS/FAIL summary to the director. Only steps that **genuinely require director-only access** — third-party UI clicks (creating a GitHub App, granting OAuth scopes), pasting secrets, browser visual judgment that requires human eyeballs, etc. — are escalated to the director.

This inverts the v0.3 default (where the director ran every step). The intent is to keep the director's day relaxing: they hear about real issues from the Project Lead, not about routine `npm test` invocations.

The producing agent fills this template out and saves it at a path under their owned deliverables tree (e.g., `backend/VERIFICATION.md`, `frontend/VERIFICATION.md`, `docs/devops-verification.md`). Multiple checklists may exist per phase.

---

## Verification Checklist — `<deliverable name>`

**Produced by:** `<agent role>`
**Phase:** `<N>`
**Project Lead executes:** all `pl-runnable` steps below (default).
**Director executes:** any step explicitly marked `director-only`.
**Gate advances when:** every step reports PASS, with proof captured by whoever ran it.

### What you are verifying

`<one paragraph — what was built, why it needs end-to-end verification on the director's actual environment, and what failure looks like>`

### Prerequisites

Before running the steps below, the runtime should have:

- [ ] `<each tool / runtime / service that needs to be available>`
- [ ] `<each env var or secret that needs to be set>`
- [ ] `<each Docker container or external service that needs running>`

If any prerequisite is missing, the Project Lead does NOT run this checklist — escalate to the director with the missing item.

### Steps

Each step is a single runnable command + the expected outcome + an explicit `kind:` annotation telling the Project Lead whether to execute it directly or route it to the director.

`kind:` values:

- `pl-runnable` — Project Lead executes via Bash / Read / Grep / etc. and captures output. **Default if omitted.**
- `director-only` — requires the director's personal action (UI click on a third-party site, paste a secret only the director has, browser visual judgment, network access the agent's sandbox doesn't have, etc.). Project Lead surfaces the step + context to the director with a clear ask.

When in doubt, mark `director-only` — over-escalation costs the director one prompt; under-escalation hides a verification gap behind a false PASS.

1. **`<command>`**
   - Kind: `pl-runnable`
   - Expected: `<exact output line, exit code, or observable side effect>`
   - On failure: `<what to check, what error class points to which root cause>`

2. **`<command>`**
   - Kind: `pl-runnable`
   - Expected: `<...>`
   - On failure: `<...>`

3. **`<browser walkthrough — Open <url> and confirm the new component renders correctly>`**
   - Kind: `director-only` *(visual judgment)*
   - Expected: `<what the director should see; ideally with a screenshot path the PL can attach to its escalation>`
   - On failure: `<what to capture if the visual is wrong>`

4. *(...)*

### Common failure modes

- **`<failure pattern 1>`** → root cause + fix path
- **`<failure pattern 2>`** → root cause + fix path

### Rollback (if the verification fails partway through)

`<exact commands to revert to a known-good state — e.g., docker compose down + drop schema + re-seed>`

### What to do on success

The Project Lead reports to the director:

```
## Project Lead Verification — <deliverable name>
**Status:** PASS
**Steps run (pl-runnable):** N of M
**Steps escalated (director-only):** K of M — director ran each, all PASS
**Notes:** <any anomalies that didn't fail but seemed worth flagging>
**Proof of execution:** <pointer to captured command output / screenshots>
```

The Project Lead then advances the gate.

---

## Why every produced-and-verified deliverable needs a checklist

In real crew runs, three patterns repeatedly cost time:

1. **The agent's sandbox lacked a tool the director has** — the agent reported success against a degraded substrate; the director's machine surfaced bugs. Without an explicit verification step, those bugs land in a later phase.
2. **Hidden state drift** — agent-installed migrations, seeded fixtures, or generated env files exist in the sandbox but not on the director's machine; subsequent live work fails until the gap is filled.
3. **Smoke testing was implicit** — no structured list, so some interactive surfaces were never visited; bugs surfaced only post-ship.

A 5–10 minute structured checklist eliminates all three classes.

## Why v0.4 inverted the default execution

In real v0.3 runs, directors found that running every checklist step manually was the largest single source of friction in an otherwise crew-driven build. Most steps (`npm install`, `npx tsc --noEmit`, `curl /health`, `psql -c 'SELECT count(*)'`, `npx vitest run`) are mechanical and the Project Lead can execute them faster and more consistently. Director attention is finite and best spent on the genuinely human-only steps (third-party UI, browser visuals, real-credential pastes) and on triaging actual failures the Project Lead surfaces.

The `kind:` annotation makes the routing explicit and structural, instead of relying on the producing agent's prose to convey what the director can/can't run.
