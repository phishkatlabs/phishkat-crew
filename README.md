# PhishKat Crew

An open-source, autonomous AI development team composed of 16 specialized Claude skills. Give the crew a goal and walk away; they will autonomously research, design, build, test, secure, document, and ship your software.

## Philosophy

PhishKat Crew uses a **Hub-and-Spoke** architecture. The `project-lead` acts as the orchestrator, and you interact solely with them. The Project Lead dispatches specialized subagents (the crew members) through 6 distinct phases of execution.

## Phases of Execution

1. **Research:** The `market-researcher` analyzes the competitor and market landscape.
2. **Design:** The `solutions-architect` converts the research into a technical system design.
3. **Build:** The `uiux-designer`, `dba`, `backend-dev`, and `frontend-dev` build the application.
4. **Verify:** The `qa-engineer`, `security-expert`, and `code-reviewer` audit the codebase.
5. **Legal:** The `legal` compliance officer ensures the product is legally compliant to launch.
6. **Ship:** The `devops` sets up deployments, and the `tech-writer` finishes documentation before the Project Lead delivers the final Ship Report.

## Installation

### For Google Vertex AI / Antigravity CLI
1. Clone this repository directly into your AI assistant's skills directory:
   ```bash
   git clone https://github.com/phishkatlabs/phishkat-crew.git ~/.gemini/antigravity/skills/phishkat-crew
   ```
2. Copy `.env.example` to `.env` and fill in your credentials.
3. Complete the `project-context.example.md` (renaming to `project-context.md`) and place it in the root of your project directory.

### For Claude Code / VSCode (Cline/RooCode)
1. Clone this repository into your global or project `.agents` directory to give your AI access to the skills:
   ```bash
   # Global installation (recommended)
   git clone https://github.com/phishkatlabs/phishkat-crew.git ~/.agents/phishkat-crew
   
   # Or project-specific installation
   git clone https://github.com/phishkatlabs/phishkat-crew.git .agents/phishkat-crew
   ```
2. Copy `.env.example` to `.env` and fill in your credentials.
3. Complete the `project-context.example.md` (renaming to `project-context.md`) and place it in the root of your project directory.

## Crew Roster

1. **Project Lead:** The Orchestrator
2. **Market Researcher:** Competitor/Market Analysis
3. **Solutions Architect:** Technical Design
4. **Growth Marketer:** SEO, Copywriting, Go-to-Market Strategy
5. **Data Analyst:** KPIs and Telemetry Engineering
6. **UI/UX Designer:** Brand and Visual System
7. **DBA:** Database Design and Management
8. **Backend Dev:** Node.js API Development
9. **Frontend Dev:** React/Vite UI Development
10. **QA Engineer:** E2E, API, and Unit Testing
11. **Security Expert:** Security Audits and Vulnerability Scanning
12. **Code Reviewer:** Pattern and Quality Audits
13. **Legal Compliance Officer:** Regulatory Compliance
14. **DevOps:** CI/CD and Containerization
15. **Tech Writer:** Documentation
16. **Community Manager:** DevRel, OSS guidelines, Launch Posts

## Dependencies

PhishKat Crew utilizes several tools to accomplish tasks. Ensure these are available to your AI assistant:

| Dependency | Purpose | Required By |
|---|---|---|
| **Firecrawl MCP** | Web scraping for competitor research | Market Researcher |
| **Playwright MCP** | Browser automation for E2E testing | QA Engineer |
| **Image Generation Tool** | Create UI mockups, icons, brand assets | UI/UX Designer |
| **GitHub CLI / API** | CI/CD setup, repository management | DevOps |
| **Docker** | Containerization | DevOps |
| **Node.js, PostgreSQL, Redis** | Core Runtime | Build Team |
