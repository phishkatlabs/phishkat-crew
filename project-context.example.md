# Project Context

## Project Name
[Insert Project Name Here]

## Competitor
[Insert Competitor/Inspiration Name and URL Here]

## Tech Stack
- **Frontend:** React 19 + Vite 6 + TypeScript
- **Backend:** Node.js 20 + Express.js + TypeScript
- **ORM:** Prisma 5.x
- **Database:** PostgreSQL 16 (schema: [insert schema name])
- **Cache:** Redis 7
- **Testing:** Playwright (E2E), Vitest (unit/integration)

## Architecture
- [Microservice / Monolith / Monorepo]
- Authentication flow: [Describe auth flow]
- Deployment strategy: [e.g., Docker deployment with docker-compose]

## Conventions
- UUID primary keys for all database tables
- `createdAt` and `updatedAt` timestamps on all tables
- User-level data isolation (filter by `userId` or `organizationId`)
- Follow Conventional Commits format
- ESLint + Prettier for code formatting
- All APIs must respond in ≤ 200ms
