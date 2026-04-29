# Agent Retry Brief

> **Purpose:** standard format for re-dispatching an agent after a prior attempt was Blocked, returned partial work, or failed verification. Carries forward what was tried + what changed so the second attempt does not repeat the failure.

The Project Lead fills this out and prepends it to the standard dispatch preamble. The retry brief is NOT a replacement for the preamble — it is additional context the agent reads first.

---

## Retry Brief — `<agent-name>` (attempt `<N>`)

You are being **re-dispatched** for a task that was attempted once and did not complete. Do not assume your prior attempt's context is in your memory — it is not.

### Prior attempt

- **Attempt ID / dispatch timestamp:** `<value>`
- **Status:** Blocked | Partial | Failed verification | Retry-after-permission-fix | Other
- **What was attempted:** `<one paragraph — what files were edited, what tools were called, how far the agent got>`
- **Why it failed:** `<root cause as best understood — permission denial, missing input, wrong scope, watchdog timeout, etc.>`
- **Artifacts produced (if any):** `<absolute paths — committed or staged. The retry MUST NOT recreate or duplicate these unless explicitly told to discard them>`

### What changed since the prior attempt

- `<list every change relevant to this task — e.g., "Firecrawl MCP was added to the user's allowlist", "the project_type field was added to project-context.md", "the architect's mount plan now exists at docs/architecture/phase-3-mount-plan.md", "decision D-NN was logged">`

### Behavior expected this time

- `<one paragraph — what success looks like, what specifically must change vs. the prior attempt>`
- **Do not redo work that was already completed** unless this brief explicitly tells you to. Read the prior artifacts and continue from where the agent stopped.
- If you encounter the same blocker again, report it WITHOUT proceeding. Do not loop.

### Hard constraints

- Same file-ownership rules as the original dispatch.
- Same `required_tools:` constraints.
- Same project-verification Step 0.
- If your prior attempt logged any decisions in `docs/decisions/`, treat them as binding — do not contradict.

---

## When NOT to use a retry brief

If the prior attempt completed successfully and the task is genuinely a new piece of work, write a fresh dispatch from scratch. Retry briefs are for resuming a single coherent task that was interrupted, not for new tasks that follow a successful one.

If the agent type itself is wrong (e.g., a Frontend bug was sent to the Backend Dev), close the wrong-type attempt and open a fresh dispatch to the correct agent — do not retry the wrong agent.
