# NutriCore

**Plataforma SaaS de gestión integral para nutricionistas.**  
Agenda · Fichas clínicas · Sesiones con IA · Automatizaciones

![Stack](https://img.shields.io/badge/Next.js_14-black?style=flat-square&logo=next.js)
![Stack](https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white)
![Stack](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Stack](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Stack](https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)
![Status](https://img.shields.io/badge/status-en_desarrollo-orange?style=flat-square)

---

## ¿Qué es NutriCore?

NutriCore digitaliza y automatiza el flujo completo de trabajo de un profesional de la nutrición:

- El paciente completa su ficha clínica desde un formulario web
- Agenda su turno y realiza el pago online
- El nutricionista recibe un dashboard antes de cada sesión con la ficha completa y una sugerencia preliminar generada por IA
- Durante la consulta puede tomar notas en tiempo real que se guardan automáticamente
- Al finalizar, el sistema envía confirmaciones, actualiza el historial y genera el resumen de evolución

---

## Stack

| Capa | Tecnología |
|---|---|
| Frontend | Next.js 14 (App Router) + TypeScript |
| Backend | NestJS + TypeScript |
| Base de datos | PostgreSQL + Prisma ORM |
| Auth | NextAuth.js + JWT |
| Tiempo real | WebSockets (NestJS Gateway) |
| IA | Claude API (Anthropic) |
| Mobile (Fase 4) | React Native + Expo |

---

## Estructura del proyecto

```
nutri-platform-core/
├── apps/
│   ├── web/                    # Next.js — interfaz nutricionista y pacientes
│   │   ├── app/
│   │   │   ├── (auth)/         # login, registro
│   │   │   ├── dashboard/      # panel principal
│   │   │   ├── patients/       # gestión de pacientes
│   │   │   ├── appointments/   # agenda
│   │   │   └── sessions/[id]/  # dashboard sesión en vivo
│   │   └── components/
│   └── api/                    # NestJS — API REST + WebSocket
│       └── src/
│           └── modules/
│               ├── auth/
│               ├── patients/
│               ├── appointments/
│               ├── sessions/
│               └── ai/
├── packages/
│   ├── database/               # Prisma schema + migraciones
│   └── shared/                 # DTOs y tipos compartidos
└── turbo.json
```

---

## Módulos principales

### Patients
CRUD completo de pacientes. Historial médico estructurado, antecedentes, objetivos del tratamiento y evolución por período.

### Appointments
Agenda de turnos con manejo de disponibilidad. Integración con Google Calendar para sincronización automática de eventos y generación de links de reunión.

### Sessions (dashboard en vivo)
Panel que el nutricionista abre antes de cada consulta. Muestra la ficha del paciente, el historial de sesiones anteriores y la sugerencia preliminar de IA. Permite tomar notas con auto-guardado via WebSocket.

### AI
Módulo que recibe la ficha del paciente y construye un prompt estructurado para el modelo. Devuelve sugerencias clínicas preliminares como contexto de apoyo. La decisión profesional siempre la toma el nutricionista.

---

## Arquitectura de un módulo

Cada módulo sigue el patrón Controller → Service → Repository:

```
patients/
├── dto/
│   ├── create-patient.dto.ts   # Validación de entrada con class-validator
│   └── update-patient.dto.ts
├── patients.controller.ts      # Solo recibe HTTP y delega al service
├── patients.service.ts         # Lógica de negocio
├── patients.repository.ts      # Acceso a datos — encapsula Prisma
└── patients.module.ts
```

---

## Cómo correr el proyecto localmente

### Requisitos

- Node.js 20+
- PostgreSQL 15+
- pnpm 8+

### Setup

```bash
# Clonar el repo
git clone https://github.com/leonelquiroga/nutri-platform-core.git
cd nutri-platform-core

# Instalar dependencias
pnpm install

# Configurar variables de entorno
cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env
# Completar los valores en ambos archivos

# Crear la base de datos y correr migraciones
pnpm db:migrate

# Seed de datos de desarrollo (opcional)
pnpm db:seed

# Levantar todos los servicios
pnpm dev
```

Los servicios quedan disponibles en:
- Web: `http://localhost:3000`
- API: `http://localhost:3001`
- Prisma Studio: `http://localhost:5555` (con `pnpm db:studio`)

---

## Variables de entorno

**API** (`apps/api/.env.example`):
```env
DATABASE_URL=postgresql://user:password@localhost:5432/nutricore
JWT_SECRET=your-secret-here
JWT_REFRESH_SECRET=your-refresh-secret-here
ANTHROPIC_API_KEY=your-key-here
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```

**Web** (`apps/web/.env.example`):
```env
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-secret-here
NEXT_PUBLIC_API_URL=http://localhost:3001
```

---

## Roadmap

- [x] Especificación técnica y arquitectura
- [ ] Setup monorepo (Turborepo + pnpm workspaces)
- [ ] Schema Prisma + migraciones base
- [ ] Módulo Auth (JWT + NextAuth)
- [ ] Módulo Patients (CRUD)
- [ ] Dashboard de sesión en vivo
- [ ] WebSocket auto-save de notas
- [ ] Módulo Appointments + disponibilidad
- [ ] Integración Google Calendar
- [ ] Módulo AI con Claude API
- [ ] React Native app (Fase 4)

---

## Contexto del proyecto

Este repositorio es la versión open source del core de NutriCore. Muestra la arquitectura y los módulos principales del sistema. Las integraciones de pagos, automatizaciones AFIP y configuraciones de producción viven en el repositorio privado del cliente.

---

## Autor

**Leonel Quiroga** — Full Stack Engineer  
[github.com/leonelquiroga](https://github.com/leonelquiroga) · [linkedin.com/in/leonelquiroga](https://linkedin.com/in/leonelquiroga) · dev.leonelquiroga@gmail.com