# PhishKat Crew

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

**PhishKat Crew** is an open-source, autonomous AI development team composed of 21 specialized agents. Give the crew a goal — "build a Sentry alternative" or "clone Linear" — and they will autonomously research the competitor, design the architecture, build the full-stack application, test it, secure it, handle legal compliance, and ship it with documentation and CI/CD.

Compatible with Claude Code, Google Vertex AI, Antigravity CLI, and RooCode (Cline).

## How It Works

PhishKat Crew uses a **Hub-and-Spoke** architecture. You interact solely with the **Project Lead**, who orchestrates 20 specialized crew members through **6 sequential phases** with **hard gate criteria** between each.

No phase advances without gate sign-off. No agent implements outside their domain.

```
You → Project Lead → Crew Members → Deliverables
```

## Design Pillars

Every crew member co-owns these principles:

1. **Feature Parity First** — Match the competitor's core value before innovating
2. **Cost-Efficient Architecture** — Minimize infrastructure cost; startup-friendly by default
3. **Developer Experience** — API-first, CLI-first, self-hostable, 10-minute onboarding
4. **Production-Grade Quality** — Secure, tested, and monitored from day one
5. **Rapid Iteration** — Ship the MVP fast, then iterate based on telemetry data

## Phases of Execution

### Phase 1: Research
The `market-researcher` analyzes the competitor's features, pricing, and user pain points. The `ux-auditor` documents the competitor's UX flows and builds a feature parity matrix.

**Gate:** Competitor analysis complete, UX audit documented, feature parity matrix with MVP scope defined.

### Phase 2: Design
The `solutions-architect` designs the system (DB schema, API contracts, component tree). The `growth-marketer` defines SEO and positioning. The `data-analyst` defines KPIs and telemetry events.

**Gate:** System design approved, no [PLACEHOLDER] tags, all decisions logged.

### Phase 3: Build
Dependency-ordered execution:
1. `uiux-designer` creates the design system (first)
2. `dba` implements the database schema (second)
3. `backend-dev`, `frontend-dev`, and `integration-engineer` build in parallel (third)

**Gate:** All endpoints responding, frontend renders, integrations functional.

### Phase 4: Verify
Parallel verification by `qa-engineer`, `security-expert`, `code-reviewer`, `performance-engineer`, `accessibility-auditor`, `data-analyst`, and `migration-specialist`. Issues enter the **Bug Loop** (max 3 fix attempts before human escalation).

**Gate:** All tests passing, >80% coverage, zero critical security findings, code review approved, load test baselines established, WCAG 2.2 AA compliance verified.

### Phase 5: Legal
The `legal` compliance officer evaluates regulatory requirements, generates legal documents, and audits third-party licenses.

**Gate:** Compliance report complete, legal documents generated, license audit clean.

### Phase 6: Ship
`devops` sets up Docker and CI/CD. `tech-writer` finalizes documentation. `growth-marketer` creates launch materials. `community-manager` builds OSS infrastructure. Project Lead compiles the **Ship Report**.

**Gate:** Docker builds, CI passes, docs complete, ship report delivered.

## Crew Roster (21 Agents)

| # | Agent | Role | Phase |
|---|-------|------|-------|
| 1 | `project-lead` | Orchestrator | All |
| 2 | `market-researcher` | Competitive Analysis | 1: Research |
| 3 | `ux-auditor` | Competitor UX Deep-Dive | 1: Research |
| 4 | `solutions-architect` | System Design | 2: Design |
| 5 | `growth-marketer` | SEO & Go-to-Market | 2: Design, 6: Ship |
| 6 | `data-analyst` | KPIs & Telemetry | 2: Design, 4: Verify |
| 7 | `uiux-designer` | Visual Design System | 3: Build |
| 8 | `dba` | Database Schema & Migrations | 3: Build |
| 9 | `backend-dev` | Express.js API | 3: Build |
| 10 | `frontend-dev` | React/Vite UI | 3: Build |
| 11 | `integration-engineer` | Webhooks & Third-Party APIs | 3: Build |
| 12 | `qa-engineer` | Testing & Coverage | 4: Verify |
| 13 | `security-expert` | Security Audit | 4: Verify |
| 14 | `code-reviewer` | Code Quality | 4: Verify |
| 15 | `performance-engineer` | Load Testing & Profiling | 4: Verify |
| 16 | `accessibility-auditor` | WCAG 2.2 Compliance | 4: Verify |
| 17 | `migration-specialist` | Data Import/Export | 4: Verify |
| 18 | `legal` | Regulatory Compliance | 5: Legal |
| 19 | `devops` | Docker & CI/CD | 6: Ship |
| 20 | `tech-writer` | Documentation | 6: Ship |
| 21 | `community-manager` | DevRel & OSS Materials | 6: Ship |

## Quick Start

### 1. Clone the crew

```bash
# Global installation (recommended for Claude Code)
git clone https://github.com/phishkatlabs/phishkat-crew.git ~/.agents/phishkat-crew

# Or project-specific
git clone https://github.com/phishkatlabs/phishkat-crew.git .agents/phishkat-crew
```

### 2. Configure your project

```bash
cp project-context.example.md project-context.md
cp .env.example .env
```

Edit `project-context.md` with your project details — the competitor you're cloning, your tech stack, your conventions, and your MVP scope. This is the most important file; the entire crew reads it.

### 3. Launch the crew

Tell the Project Lead what to build:

> "Build an open-source alternative to Sentry for error monitoring. Target solo developers and small startups. Must be self-hostable with Docker."

The Project Lead handles everything from there.

## Templates

| Template | Purpose |
|----------|---------|
| `templates/competitor-analysis.md` | Market research output format |
| `templates/feature-parity-matrix.md` | Feature comparison and MVP scoping |
| `templates/ship-report.md` | Final delivery report |
| `templates/compliance-report.md` | Legal compliance assessment |
| `templates/decisions/` | Architectural decision log (one file per decision) |
| `templates/bug-report.md` | Structured bug reporting |
| `templates/phase-gate-checklist.md` | Phase transition sign-off |
| `templates/phase-3-mount-plan.md` | File-ownership and integration handoff plan for Phase 3 build |
| `templates/deployment.md` | First-time production deployment runbook |

## Dependencies

| Dependency | Purpose | Required By |
|---|---|---|
| **Firecrawl MCP** | Web scraping for competitor research | Market Researcher, UX Auditor |
| **Playwright MCP** | Browser automation for E2E testing | QA Engineer |
| **axe-core / @axe-core/playwright** | Automated WCAG 2.2 accessibility testing | Accessibility Auditor |
| **k6 or Artillery** | Load testing and performance benchmarking | Performance Engineer |
| **Image Generation Tool** | UI mockups, logos, brand assets | UI/UX Designer |
| **GitHub CLI / API** | CI/CD setup, repository management | DevOps |
| **Docker** | Containerization | DevOps |
| **Node.js 20, PostgreSQL 16, Redis 7** | Core runtime | Build Team |

## Project Structure

```
phishkat-crew/
├── CLAUDE.md                    # Orchestration protocol & design pillars
├── project-context.example.md   # Project specification template
├── .env.example                 # Environment variables template
├── project-lead/SKILL.md        # Orchestrator
├── market-researcher/SKILL.md   # Phase 1
├── ux-auditor/SKILL.md          # Phase 1
├── solutions-architect/SKILL.md # Phase 2
├── growth-marketer/SKILL.md     # Phase 2 + 6
├── data-analyst/SKILL.md        # Phase 2 + 4
├── uiux-designer/SKILL.md       # Phase 3
├── dba/SKILL.md                 # Phase 3
├── backend-dev/SKILL.md         # Phase 3
├── frontend-dev/SKILL.md        # Phase 3
├── integration-engineer/SKILL.md # Phase 3
├── qa-engineer/SKILL.md         # Phase 4
├── security-expert/SKILL.md     # Phase 4
├── code-reviewer/SKILL.md       # Phase 4
├── performance-engineer/SKILL.md # Phase 4
├── accessibility-auditor/SKILL.md # Phase 4
├── migration-specialist/SKILL.md # Phase 4
├── legal/SKILL.md               # Phase 5
├── devops/SKILL.md              # Phase 6
├── tech-writer/SKILL.md         # Phase 6
├── community-manager/SKILL.md   # Phase 6
└── templates/                   # Output format templates
    ├── competitor-analysis.md
    ├── feature-parity-matrix.md
    ├── ship-report.md
    ├── compliance-report.md
    ├── decisions/               # one-file-per-decision template + README
    ├── bug-report.md
    ├── phase-gate-checklist.md
    ├── phase-3-mount-plan.md
    └── deployment.md
```

## Contributing

PRs welcome! See the agent SKILL.md files for the format if you want to add new crew members.

## License

MIT
