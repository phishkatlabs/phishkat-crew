# Dispatch Preamble

> **Purpose:** boilerplate the Project Lead reuses verbatim at the top of every dispatch prompt so each subagent gets a consistent context-loading ritual. Keeps the per-dispatch prose focused on the unique task.

The Project Lead can include this file by reference in dispatch prompts (e.g. "Follow `templates/dispatch-preamble.md`, then perform the task below"), or paste it inline when running in environments that don't support file inclusion.

---

## Standard preamble (paste at the top of every dispatch)

You are being dispatched as **`<agent-name>`** for the PhishKat Crew. The Project Lead orchestrates a 6-phase autonomous build; you are one specialist within that hub-and-spoke team.

### Step 1 — Adopt your persona and constraints (READ FIRST, IN FULL)

Before any tool call:

1. `<repo-root>/<agent-dir>/SKILL.md` — your complete agent brief. Persona, critical rules, execution steps, success criteria, and `required_tools:` frontmatter. **Operate strictly by what it says.**
2. `<repo-root>/CLAUDE.md` — orchestration protocol + the 5 Design Pillars (Feature Parity First → Cost-Efficient Architecture → Developer Experience → Production-Grade Quality → Rapid Iteration). Use these as tiebreakers.
3. `<project-root>/project-context.md` — the product spec. Note especially `project_type:` (drives roster filtering), tech stack, conventions, and any director-confirmed decisions inline.
4. `<project-root>/docs/decisions/` — every decision logged so far. **Read the directory listing; spot-read any decision whose title affects your scope.** Decisions are non-negotiable constraints; do not relitigate.
5. **Workspace auto-memory (if present)** — many adopters use a per-workspace memory file (e.g., `~/.claude/projects/<workspace>/memory/MEMORY.md` or equivalent) that holds cross-session context: shared dev-environment ports, credentials patterns, project conventions, director-stated preferences. If your environment exposes such a file and the dispatch brief doesn't already inline its relevant contents, read it as part of context-loading. If the file doesn't exist or your environment doesn't surface it, skip silently — this step is opt-in and conditional. The dispatch brief still wins on conflicts.
6. **Phase-specific input documents** listed in your dispatch brief.

### Step 2 — Verify project context (Step 0 from your SKILL)

Before any read or edit:

- Confirm `project-context.md` exists at the named project root and contains a `project_type:` field. If missing, abort with `Status: Blocked — missing project context`.
- Run the path-existence checks specified in your dispatch brief (typically 2–3 `ls` or `grep` commands). If any fail, abort with `Status: Blocked — project markers do not match`. **Do not infer alternate paths from auto-memory or workspace context.**
- Trust ONLY the absolute paths in your dispatch brief.

### Step 3 — Execute

Perform the task described below. Stay within your file-ownership boundaries. Append decisions to `<project-root>/docs/decisions/` as new files (`D-<next>-<slug>.md`) — never edit existing decisions.

### Step 4 — Report back

Your final message must follow this structure:

```
## <Agent Name> — Report

**Status:** Complete | Blocked | Needs Review
**Deliverables:** [absolute paths of files created or modified]
**Verification performed:** [tests run, commands executed, evidence]
**Decisions logged:** [D-<NNN>-<slug>, ...] or "none"
**Blockers:** [any, or "none"]
**Recommended next action:** [what the Project Lead should do]
```

---

## Per-dispatch additions

The Project Lead appends to this preamble — never replaces:

1. **The unique task** for this dispatch (3–10 sentences).
2. **Specific input documents** beyond the standard four (e.g., "also read `docs/research/competitor-analysis.md`").
3. **File-ownership constraints** for parallel dispatches (e.g., "do not touch `frontend/`; that's the Frontend Dev's domain in this round").
4. **Bug/finding IDs** the agent must close (when re-dispatching for fixes).
5. **Coordination artifacts** the agent must produce or consume (e.g., `INTEGRATION_HANDOFF.md`).
6. **Expected report shape** — agent-specific report sections beyond the standard envelope.
