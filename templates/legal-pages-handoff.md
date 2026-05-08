# Legal Pages Handoff (Phase 3 → Phase 5)

> **Purpose:** codify the contract between Phase 3 Frontend Dev and Phase 5 Legal Compliance Officer so the SPA's legal-page surface (footer, routes, page components) is **pre-wired during Phase 3** and Phase 5 only has to write the policy text. The director never has to manually wire a footer or hand-link policy pages.

## Why this contract exists

Phase 5 (Legal) runs after Phase 3 (Build) in the standard crew sequence. Without an explicit handoff, two things go wrong:

1. **Phase 5 produces policy documents that aren't reachable from the SPA.** The footer, routes, and page components don't exist yet because Phase 3 didn't know they were needed. GDPR Art. 12 transparency fails at first page load. The crew either dispatches a Frontend Dev follow-up between Phase 5 and Phase 6 (delaying the gate), or the director manually wires the SPA after Phase 5 — both failure modes burn director attention.
2. **Phase 5 invents its own output paths.** Without a fixed-path contract, the Legal officer might write `docs/legal/privacy.md` while the frontend imports `docs/legal/privacy-policy.md` — drift that's invisible until prod.

This template makes the contract bilateral and explicit.

## The contract

### Phase 3 Frontend Dev pre-wires the surface

The Frontend Dev SKILL includes a mandatory step: **"Scaffold the legal-pages surface."** Specifically:

1. **Create a `<LegalFooter />` component** (or whatever the project's component naming convention is). Mount it in the app shell so it appears on every authenticated page, AND on the public login/signup screen so first-page-load GDPR Art. 12 transparency is satisfied. Footer content: 4 inline links to `/privacy`, `/terms`, `/eula`, `/cookies`, plus a small `© <COMPANY_LEGAL_NAME>` line (literal placeholder; director fills before publish). Style: low-emphasis text-secondary color, underlined links per WCAG SC 1.4.1 (don't rely on color alone), 24×24px touch targets per SC 2.5.8.

2. **Add 4 public routes** OUTSIDE the project's auth gate: `/privacy`, `/terms`, `/eula`, `/cookies`.

3. **Create 4 page components** (e.g., `pages/legal/PrivacyPolicy.tsx` etc.). Each imports the corresponding markdown file at a fixed path via the build tool's raw-string import (Vite `?raw`, webpack `raw-loader`, etc.) and renders it with a Markdown library (e.g. `react-markdown` for React-based stacks). Render inside a centered narrow column (~720px max-width) with reading-comfortable line-height.

4. **Create the 4 markdown files at fixed paths** (these are the canonical paths Phase 5 fills):
   - `docs/legal/privacy-policy.md`
   - `docs/legal/terms-of-service.md`
   - `docs/legal/eula.md`
   - `docs/legal/cookie-policy.md`

   Each starts as a **skeletal placeholder** with a real top-level heading and a single body line:

   ```markdown
   # Privacy Policy

   > _This is placeholder content. Phase 5 Legal Compliance Officer will fill in the real policy text. The SPA imports this file via raw-string import; updating the content here updates the live page on next build._
   ```

   The skeletal shape ensures the visual layout doesn't shift dramatically when Phase 5 fills in real content.

5. **Build-tool config (if needed):** for Vite-based projects, lift `server.fs.allow` to the project root so `?raw` imports of `../../docs/legal/*.md` work in dev. For other build tools, configure the equivalent.

6. **Extend the project's a11y test spec** (e.g., `src/__tests__/e2e/a11y.spec.ts`) to include the 4 new public routes as `isPublic: true` so axe-core scans them without auth.

### Phase 5 Legal Compliance Officer fills the text

The Legal officer's `Outputs` section explicitly notes: **the four user-facing policy file paths are fixed; the frontend pre-wires them; do NOT move or rename.** The officer overwrites the placeholder content with real legal prose. The SPA picks up the new content on the next build with **zero UI changes required**.

The other Phase 5 outputs (`docs/legal/compliance-report.md`, `LICENSES.md`) are not pre-wired into the UI — those are internal/operator-facing documents and Phase 5 owns their paths fully.

### Verification

After Phase 5 completes, the standard `verification-checklist.md` ritual confirms the integration:

- One `pl-runnable` step: `npm run build` succeeds (the build tool resolves the markdown imports against the now-real content)
- One `pl-runnable` step: HTTP probe to `/privacy`, `/terms`, `/eula`, `/cookies` from a logged-out session returns 200 (routes are public)
- One `pl-runnable` step: extended a11y spec passes — confirms the legal pages still meet WCAG with the longer real text
- One `director-only` step (visual): the director clicks each footer link from a few different pages and confirms the rendered prose looks correct

Without the pre-wiring contract, this verification would surface an integration gap in Phase 5; with the contract, it confirms the (already-working) integration end-to-end.

## When this handoff does NOT apply

- **`project_type: open-source-library`** — typically no SPA. The legal surface is just `LICENSES.md` and possibly a project-level `LICENSE`. Skip the footer/routes step; just emit the markdown files.
- **`project_type: enterprise-on-prem`** — packaging-dependent. If the on-prem deployment includes a web admin UI, follow the contract for that UI. If not, route the legal docs into the deployment guide instead.
- **Headless / API-only products** — no UI surface to pre-wire. Phase 5 emits markdown only; reference them in API docs (e.g., a "Legal" section in `API.md` linking to the markdown files in the repo).

The Project Lead applies judgment when scoping the Frontend Dev's "scaffold the legal surface" step based on `project_type` and the actual UI surface.
