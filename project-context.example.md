# Project Context

This file is the single source of truth for what the crew is building. Copy this to `project-context.md`, fill in every section, and place it in the root of your project directory. The crew reads this file at every phase.

---

## Project Name
[Your product name]

## One-Line Description
[What does this product do in one sentence?]

## Competitor
[The product you're building an alternative to]

- **Name:** [e.g., Sentry, Linear, Postman]
- **URL:** [e.g., https://sentry.io]
- **Pricing page:** [e.g., https://sentry.io/pricing]
- **Why users want an alternative:** [e.g., too expensive for startups, missing self-host option, poor DX]

## Target Audience
[Who is this for? Be specific.]
- **Primary:** [e.g., Solo developers and small startups (1-10 engineers)]
- **Secondary:** [e.g., Open-source maintainers who need free tooling]

## Differentiation Strategy
[How will your product stand out from the competitor?]
- **Price:** [e.g., Free tier + $10/mo vs competitor's $29/mo minimum]
- **Feature:** [e.g., Self-hostable with Docker, CLI-first workflow]
- **Experience:** [e.g., Simpler UX, faster onboarding, better docs]

---

## Tech Stack

### Frontend
- **Framework:** React 19 + TypeScript
- **Build Tool:** Vite 6
- **State Management:** TanStack Query v5
- **Icons:** Lucide React
- **Styling:** Custom CSS *(set to "Tailwind CSS" if preferred)*
- **Testing:** Playwright (E2E)

### Backend
- **Runtime:** Node.js 20
- **Framework:** Express.js + TypeScript
- **ORM:** Prisma 5.x
- **Authentication:** JWT (HS256) *(describe auth flow: self-managed, OAuth, or integration with existing auth service)*
- **Security:** helmet, cors, express-rate-limit
- **Testing:** Vitest (unit/integration), Supertest (API)

### Database & Cache
- **Database:** PostgreSQL 16
- **Schema name:** [e.g., "billing" — used for schema-per-service isolation]
- **Cache:** Redis 7 *(remove if not needed)*

### Infrastructure
- **Containerization:** Docker + Docker Compose
- **CI/CD:** GitHub Actions
- **Reverse Proxy:** nginx *(remove if not needed)*
- **Hosting:** [e.g., Self-hosted on personal server, AWS, Vercel]

---

## Architecture

- **Pattern:** [Monolith / Microservice / Monorepo]
- **Authentication flow:** [e.g., "JWT tokens issued by separate auth service at auth.example.com; this service validates tokens using shared JWT_ACCESS_SECRET"]
- **Deployment strategy:** [e.g., "Docker container deployed via SSH to personal server"]

### Existing Services (if any)
[List any existing services this product integrates with. Remove this section if building standalone.]

| Service | URL (dev) | URL (prod) | Port | Purpose |
|---------|-----------|------------|------|---------|
| [e.g., Auth Service] | [http://localhost:4000] | [https://auth.example.com] | [4000] | [SSO authentication] |
| [e.g., Error Monitoring] | [http://localhost:4001] | [https://errors.example.com] | [4001] | [Error tracking] |

### Shared Infrastructure
[Describe shared databases, networks, secrets, etc. Remove if standalone.]
- **Database:** [e.g., Shared PostgreSQL instance, schema-per-service]
- **Docker network:** [e.g., `my-network` (external, created by infrastructure service)]
- **Shared secrets:** [e.g., JWT_ACCESS_SECRET shared across services]

---

## Conventions

### Database
- UUID primary keys for all tables (`@id @default(uuid())`)
- `createdAt DateTime @default(now())` and `updatedAt DateTime @updatedAt` on all models
- User-level data isolation (filter by `userId` or `organizationId` on every query)
- Schema name must match service name for shared database deployments

### Code
- TypeScript strict mode — zero `any` types
- Conventional Commits format for all commits
- ESLint + Prettier for code formatting
- Functions under 50 lines, single responsibility

### API
- RESTful endpoints following standard HTTP methods and status codes
- All endpoints must target ≤200ms response time (p95)
- JWT authentication on all non-public endpoints
- Rate limiting on all public endpoints
- Audit logging for sensitive operations (create, update, delete, permission changes)

### Testing
- >80% line coverage, >75% branch coverage
- Test pyramid: unit → integration → API → E2E
- No `.skip` tests without linked issues
- No hardcoded test data — use factories/fixtures
- No sleep/setTimeout in tests — condition-based waits only

---

## Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname?schema=schemaname

# Redis (remove if not needed)
REDIS_URL=redis://:password@localhost:6379

# Authentication
JWT_ACCESS_SECRET=your-jwt-secret-here
# JWT_REFRESH_SECRET=your-refresh-secret-here  # if using refresh tokens

# Application
PORT=3000
NODE_ENV=development
CORS_ORIGIN=http://localhost:3000
FRONTEND_URL=http://localhost:3000

# External Services (add as needed)
# SMTP_HOST=smtp.gmail.com
# SMTP_USER=your-email@gmail.com
# SMTP_PASS=your-app-password
# DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/...
# SLACK_WEBHOOK_URL=https://hooks.slack.com/services/...
```

---

## Scope Notes

### MVP Must-Haves
[List the features that MUST be in the first release. Be specific.]
1. [e.g., User authentication (login, register, password reset)]
2. [e.g., Project CRUD operations]
3. [e.g., Dashboard with key metrics]

### Explicitly Out of Scope (for now)
[List features you are NOT building yet to prevent scope creep.]
1. [e.g., Multi-tenancy / organization support]
2. [e.g., Mobile app]
3. [e.g., Real-time collaboration]

### Migration from Competitor
[Does the product need to import data from the competitor?]
- **Import formats needed:** [CSV, JSON, competitor API, none]
- **Key data to migrate:** [e.g., projects, issues, error events]
- **Priority:** [MVP / V2 / Not needed]
