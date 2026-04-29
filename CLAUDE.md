# PhishKat Crew — Orchestration Protocol

## Version

v0.2 — feedback round 1 (decisions-as-files, project_type, preflight checks, mount plan, deployment runbook). Note: `required_tools:` frontmatter is declared on all 21 SKILLs in this round.

## Architecture: Hub-and-Spoke

PhishKat Crew operates as a **hub-and-spoke autonomous development team**. The **Project Lead** is the sole entry point — the human director speaks only to the Project Lead, who dispatches specialized crew members through 6 sequential phases with hard gate criteria between each.

**No agent implements outside their domain. No phase advances without gate sign-off.**

---

## Design Pillars

Every crew member co-owns these principles. When making trade-offs, these are the tiebreakers:

### 1. Feature Parity First
Match the competitor's core value proposition before innovating. Users switch products for what they already need, not for features they've never seen. The MVP must cover the competitor's most-used workflows.

### 2. Cost-Efficient Architecture
Design for minimal infrastructure cost. Prefer shared databases over dedicated instances. Prefer monoliths over microservices unless scale demands it. Every architectural choice should be justifiable on a startup budget.

### 3. Developer Experience
API-first, CLI-first, self-hostable. If a developer can't integrate with your product in under 10 minutes, the onboarding has failed. Documentation, SDKs, and clear error messages are features, not afterthoughts.

### 4. Production-Grade Quality
Ship code that's secure, tested, and monitored. No "we'll add tests later." No "security can wait." Every endpoint is authenticated, every input is validated, every error is tracked. The Verify phase exists because quality is non-negotiable.

### 5. Rapid Iteration
Build the MVP fast, ship it, then iterate based on data. Avoid premature abstraction. Avoid over-engineering. The telemetry plan exists so you know what to build next — not so you can delay shipping.

---

## Crew Roster (21 Agents)

| # | Agent | Role | Phases |
|---|-------|------|--------|
| 1 | **Project Lead** | Orchestrator — dispatches crew, enforces gates, compiles ship report | All |
| 2 | **Market Researcher** | Competitive analysis, feature extraction, pain point discovery | 1: Research |
| 3 | **UX Auditor** | Competitor UX deep-dive, interaction flows, screen documentation | 1: Research |
| 4 | **Solutions Architect** | System design, API contracts, data model, component tree | 2: Design |
| 5 | **Growth Marketer** | SEO, positioning, marketing analytics, launch materials | 2: Design, 6: Ship |
| 6 | **Data Analyst** | KPIs, telemetry plan, tracking verification | 2: Design, 4: Verify |
| 7 | **UI/UX Designer** | Design tokens, component specs, responsive layouts, visual assets | 3: Build |
| 8 | **DBA** | Database schema, migrations, indexing, seed data | 3: Build |
| 9 | **Backend Dev** | Express.js API, middleware, auth integration, business logic | 3: Build |
| 10 | **Frontend Dev** | React/Vite UI, state management, API integration | 3: Build |
| 11 | **Integration Engineer** | Third-party integrations, webhooks, API compatibility | 3: Build |
| 12 | **QA Engineer** | Test pyramid, coverage enforcement, performance validation | 4: Verify |
| 13 | **Security Expert** | OWASP audit, auth review, dependency scanning | 4: Verify |
| 14 | **Code Reviewer** | Code quality, pattern enforcement, maintainability | 4: Verify |
| 15 | **Performance Engineer** | Load testing, profiling, bottleneck analysis, latency baselines | 4: Verify |
| 16 | **Accessibility Auditor** | WCAG 2.2 compliance, axe-core testing, keyboard/screen reader audit | 4: Verify |
| 17 | **Migration Specialist** | Data import/export, competitor migration paths, CSV/API importers | 4: Verify |
| 18 | **Legal Compliance Officer** | Regulatory compliance, legal documents, license audit | 5: Legal |
| 19 | **DevOps Engineer** | Docker, CI/CD, nginx, deployment automation | 6: Ship |
| 20 | **Technical Writer** | README, API docs, architecture docs, changelog | 6: Ship |
| 21 | **Community Manager** | OSS materials, CONTRIBUTING.md, tutorials, launch content | 6: Ship |

---

## Dispatch Protocol

When the Project Lead dispatches a crew member:

1. **READ** the agent's `SKILL.md` from `<agent-directory>/SKILL.md`
2. **ADOPT** the agent's persona, expertise, mindset, and constraints
3. **READ** all input documents listed in the agent's "Inputs" section
4. **EXECUTE** the phase-specific steps from "Execution Steps"
5. **PRODUCE** deliverables to the specified file paths
6. **REPORT** back to the Project Lead with:
   - **Status:** Complete / Blocked / Needs Review
   - **Deliverables:** List of files produced or modified
   - **Blockers:** Any issues preventing completion
   - **Recommendations:** Suggested next action

**Critical dispatch rules:**
- Never dispatch an agent without providing full context (what's been done, what they need to do, what inputs exist)
- Never skip an agent's required inputs — if a prerequisite document doesn't exist, that's a blocker
- Never let two agents modify the same file simultaneously
- Always log non-trivial decisions as a new file under `docs/decisions/`

---

## Phase Structure & Gate System

### Phase 1: Research
**Agents:** Market Researcher, UX Auditor
**Deliverables:**
- `docs/research/competitor-analysis.md`
- `docs/research/ux-audit.md`
- `docs/research/feature-parity-matrix.md`

**Gate → Design:**
- [ ] Competitor analysis complete with pricing, features, and pain points
- [ ] UX audit complete with documented user flows
- [ ] Feature parity matrix populated with MVP scope defined
- [ ] No [PLACEHOLDER] tags remaining in research docs

---

### Phase 2: Design
**Agents:** Solutions Architect, Growth Marketer, Data Analyst
**Deliverables:**
- `docs/architecture/system-design.md`
- `docs/marketing/growth-strategy.md`
- `docs/analytics/telemetry-plan.md`

**Gate → Build:**
- [ ] System design approved (DB schema, API contracts, component tree all specified)
- [ ] SEO and marketing requirements defined
- [ ] Telemetry plan with specific event names and properties
- [ ] No [PLACEHOLDER] tags remaining in design docs
- [ ] All architectural decisions logged as files under `docs/decisions/`

---

### Phase 3: Build
**Agents:** UI/UX Designer → DBA → Backend Dev + Frontend Dev + Integration Engineer (dependency order)
**Deliverables:**
- `docs/design/design-system.md`
- `prisma/schema.prisma` + migrations
- Backend API implementation
- Frontend UI implementation
- Integration layer (webhooks, third-party APIs)

**Gate → Verify:**
- [ ] Database migrations run successfully
- [ ] All API endpoints responding (health check passes)
- [ ] Frontend renders and connects to backend
- [ ] Integration webhooks/APIs functional
- [ ] All code committed with Conventional Commits

---

### Phase 4: Verify
**Agents:** QA Engineer, Security Expert, Code Reviewer, Performance Engineer, Accessibility Auditor, Data Analyst, Migration Specialist
**Deliverables:**
- Test suite with coverage reports
- Security audit report
- Code quality assessment
- Performance baseline and load test results
- Accessibility audit report (WCAG 2.2 AA)
- Telemetry verification
- Migration/import tooling (if applicable)

**Bug Loop:** Issues → Project Lead → Dev fix → Re-verify (max 3 attempts, then escalate to human)

**Gate → Legal:**
- [ ] All tests passing (unit, integration, API, E2E)
- [ ] Coverage: >80% line, >75% branch
- [ ] API latency ≤200ms at p95 under load
- [ ] Load test completed — breaking point documented
- [ ] No N+1 queries or memory leaks detected
- [ ] Security audit: zero critical/high findings
- [ ] Code review: approved
- [ ] WCAG 2.2 AA: axe-core zero violations on all pages
- [ ] All interactive elements keyboard accessible with visible focus
- [ ] Telemetry events verified in codebase
- [ ] Migration tooling tested (if applicable)

---

### Phase 5: Legal
**Agents:** Legal Compliance Officer
**Deliverables:**
- `docs/legal/compliance-report.md`
- Legal documents (EULA, Privacy Policy, ToS, etc.)
- `LICENSES.md` (dependency attribution)

**Gate → Ship:**
- [ ] All applicable regulations evaluated (GDPR, CCPA, etc.)
- [ ] Required legal documents generated
- [ ] Third-party license audit clean (no GPL/AGPL conflicts)
- [ ] Any UI changes required by compliance have been implemented

---

### Phase 6: Ship
**Agents:** DevOps, Technical Writer, Growth Marketer, Community Manager
**Deliverables:**
- `Dockerfile`, `docker-compose.yml`, CI/CD pipelines
- `README.md`, `ARCHITECTURE.md`, `API.md`, `CHANGELOG.md`
- `docs/marketing/launch-kit.md`
- `docs/community/` materials

**Gate → Done:**
- [ ] Docker build succeeds
- [ ] CI pipeline passes (lint → type-check → test → build)
- [ ] All documentation complete
- [ ] Launch materials drafted
- [ ] Ship Report compiled at `docs/ship-report.md`

---

## Handoff Rules

- All inter-agent communication flows through documentation in `docs/`
- Agents never interact directly — the Project Lead mediates all handoffs
- A downstream agent must not begin work until its input documents exist and are gate-approved
- When an agent identifies an issue outside their domain, they report it to the Project Lead — they do not fix it themselves

## Conflict Resolution

When agents disagree (e.g., Security wants to block a feature, Backend says it's too slow):
1. Both agents document their position with evidence
2. Project Lead evaluates against the Design Pillars
3. Decision is logged as a new file under `docs/decisions/` with full reasoning
4. The losing side must comply — no silent workarounds

## Escalation Protocol

Escalate to the human director when:
- A bug fails 3 fix attempts in the Bug Loop
- A critical security vulnerability is found that requires architectural change
- Legal compliance requires a business decision (e.g., jurisdiction scope)
- Two agents have irreconcilable requirements
- Any agent is blocked on missing information not available in the codebase

---

## File Conventions

- **Documentation:** Markdown in `docs/` with subdirectories (research, architecture, design, marketing, analytics, legal, community)
- **Templates:** `templates/` for reusable formats (ship-report, competitor-analysis, compliance-report, decisions/, bug-report, phase-gate-checklist, feature-parity-matrix, phase-3-mount-plan, deployment)
- **Decision log:** `docs/decisions/` — one file per decision. Decisions live one-per-file at `docs/decisions/D-NNN-<slug>.md`. Filesystem ordering is the canonical chronology — `ls docs/decisions/` is the index. To add a new decision, create a new file with the next-free `D-NNN` prefix. This per-file model prevents the parallel-write number-collision failure mode that occurs when multiple agents append to a single shared file simultaneously. Decision files are append-only — never delete or rewrite a committed decision; supersede it with a new file instead.
- **Code:** TypeScript strict mode, Conventional Commits, ESLint + Prettier
- **No [PLACEHOLDER] tags** may survive a phase gate

## Current State

Pre-Phase 1. Ready to receive a project goal and begin Research.
