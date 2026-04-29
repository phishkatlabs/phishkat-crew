# Decision Log — One File Per Decision

> **Note to Crew:** Decisions live one-per-file in this directory. There is no shared `decisions.md` to append to.

## Why one file per decision?

Two or more agents working in parallel will both try to append to a shared `decisions.md` and pick the same next-free `D-NNN` number, then merge over each other. Splitting decisions into separate files eliminates the collision because each agent writes a different file.

## File naming

```
docs/decisions/D-NNN-<short-slug>.md
```

- `NNN` is a zero-padded sequential number starting at `001`
- `<short-slug>` is a kebab-case summary (e.g. `D-007-postgres-over-mongodb.md`)
- Filesystem ordering by name is the canonical chronology — never renumber a decision once it has been committed
- Superseding a decision does NOT delete the file; the superseded entry is updated to reference its replacement, and the new file links back

## How to add a decision

1. List the existing files in `docs/decisions/` to find the highest `D-NNN` in use
2. Create `D-<next>-<slug>.md` using the template at `templates/decisions/D-NNN-example-decision.md`
3. Fill in every field — date, author, status, context, options, choice, reasoning
4. Commit the file as part of the same change that motivated the decision
5. Run `scripts/regenerate-decisions-index.sh` to refresh `docs/decisions/INDEX.md`

## Status field semantics

- `accepted` — the decision is in force
- `superseded by D-NNN` — a later decision replaced this one; link to the replacement
- `reversed by D-NNN` — a later decision undid this one without a direct replacement

## Linking decisions

When a decision references another (supersedes, depends on, conflicts with), use a relative markdown link to the target file (e.g. `[D-014](./D-014-roster-adjustment.md)`). Do not duplicate the target's content — link only.

## Index regeneration

`scripts/regenerate-decisions-index.sh` walks this directory, extracts the title and date from each `D-NNN-*.md`, and writes a sorted table to `docs/decisions/INDEX.md`. The script is idempotent and safe to run repeatedly.
