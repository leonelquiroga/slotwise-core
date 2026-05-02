# Slotwise

**Open-source core of a SaaS platform for service professionals.**  
Appointments · Client records · AI-assisted sessions · Payments · Automations

![Stack](https://img.shields.io/badge/Next.js_14-black?style=flat-square&logo=next.js)
![Stack](https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white)
![Stack](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Stack](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Stack](https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)
![Status](https://img.shields.io/badge/status-in_development-orange?style=flat-square)

---

## What is Slotwise?

Slotwise is a white-label SaaS platform for any professional who works by appointment.  
A nutritionist, a psychologist, a personal trainer, a veterinarian, a dog groomer — any service-based business that needs to manage clients, schedule sessions, collect payments, and keep structured records.

The platform adapts to each business through configuration, not code changes.

**Core workflow:**

1. Client fills in their intake form (fields are configurable per business type)
2. Client books a slot and pays online
3. Professional receives a pre-session dashboard with the complete client file and an AI-generated context summary
4. Notes taken during the session are auto-saved in real time
5. Post-session: payment is logged, invoicing automation triggered, client record updated

---

## Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14 (App Router) + TypeScript |
| Backend | NestJS + TypeScript |
| Database | PostgreSQL + Prisma ORM |
| Auth | NextAuth.js + JWT |
| Real-time | WebSockets (NestJS Gateway) |
| AI | Claude API (Anthropic) |
| Mobile (Phase 4) | React Native + Expo |

---

## Architecture

Slotwise is a **pnpm monorepo** managed with Turborepo:

```
slotwise-core/
├── apps/
│   ├── web/                      # Next.js — professional portal + client portal
│   │   └── app/
│   │       ├── (auth)/           # login, register, onboarding
│   │       ├── dashboard/        # main panel
│   │       ├── clients/          # client management
│   │       ├── appointments/     # calendar and scheduling
│   │       └── sessions/[id]/    # live session dashboard
│   └── api/                      # NestJS — REST API + WebSocket
│       └── src/
│           └── modules/
│               ├── auth/         # JWT, guards, strategies
│               ├── tenants/      # multi-tenancy, business config
│               ├── clients/      # client records (replaces "patients")
│               ├── appointments/ # scheduling logic
│               ├── sessions/     # live session, notes, WebSocket
│               └── ai/           # Claude API integration
├── packages/
│   ├── database/                 # Prisma schema + migrations
│   └── shared/                   # DTOs, types, constants
└── turbo.json
```

### Module pattern

Every NestJS module follows a strict Controller → Service → Repository layering:

```
clients/
├── dto/
│   ├── create-client.dto.ts     # Input validation (class-validator)
│   └── update-client.dto.ts
├── clients.controller.ts        # HTTP layer — receives, delegates, responds
├── clients.service.ts           # Business logic — no HTTP knowledge
├── clients.repository.ts        # Data access — encapsulates Prisma
└── clients.module.ts
```

---

## Multi-tenancy

Each registered business is a **Tenant**. The Tenant has a `businessType` that controls:

- Labels shown in the UI ("Patient" vs "Client" vs "Pet owner")
- Fields available in the intake form (configurable JSON schema)
- Default session duration
- Which features are enabled (AI summary, payments, automations)

This means the same codebase serves a nutritionist and a dog grooming salon without a single `if` in the domain logic.

---

## Running locally

### Requirements

- Node.js 20+
- PostgreSQL 15+
- pnpm 8+

### Setup

```bash
git clone https://github.com/leonelquiroga/slotwise-core.git
cd slotwise-core

pnpm install

cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env
# Fill in the values

pnpm db:migrate
pnpm db:seed     # optional — loads demo data

pnpm dev
```

| Service | URL |
|---|---|
| Web | http://localhost:3000 |
| API | http://localhost:3001 |
| Prisma Studio | http://localhost:5555 (`pnpm db:studio`) |

---

## Environment variables

**API** (`apps/api/.env.example`):

```env
DATABASE_URL=postgresql://user:password@localhost:5432/slotwise
JWT_SECRET=
JWT_REFRESH_SECRET=
ANTHROPIC_API_KEY=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```

**Web** (`apps/web/.env.example`):

```env
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=
NEXT_PUBLIC_API_URL=http://localhost:3001
```

---

## Roadmap

- [x] Technical specification and architecture design
- [x] Public repository setup
- [ ] Monorepo scaffold (Turborepo + pnpm workspaces)
- [ ] Prisma schema + base migrations
- [ ] Auth module (JWT + NextAuth)
- [ ] Tenants module (multi-tenancy, business config)
- [ ] Clients module (CRUD + configurable fields)
- [ ] Appointments module (scheduling + availability)
- [ ] Live session dashboard (WebSocket auto-save)
- [ ] AI module (Claude API integration)
- [ ] React Native app (Phase 4)

---

## About this repository

This is the open-source core of Slotwise. It covers the main architectural modules and patterns. Payment integrations, invoicing automations, and production configuration live in the private repository.

---

## Author

**Leonel Quiroga** — Full Stack Engineer  
[github.com/leonelquiroga](https://github.com/leonelquiroga) · [linkedin.com/in/leonelquiroga](https://linkedin.com/in/leonelquiroga) · dev.leonelquiroga@gmail.com