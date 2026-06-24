# KEYVERA Real Estate Brokerage

A full-stack luxury bilingual (Arabic/English) real estate website for KEYVERA Real Estate Brokerage, Alexandria, Egypt. Competes with Sotheby's, Emaar, and DAMAC in design quality.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080, path `/api`)
- `pnpm --filter @workspace/keyvera run dev` — run the frontend (path `/`)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Seed Accounts

- **Owner:** nadeem@keyvera.com / Nadeem@2024
- **CEO:** doaa@keyvera.com / Doaa@2024
- **Staff:** staff@keyvera.com / Staff@2024

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, wouter, Framer Motion, Tailwind CSS v4, shadcn/ui
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — source of truth for all API contracts
- `lib/db/src/schema/` — DB schema (users, properties, projects, media, inquiries)
- `artifacts/api-server/src/routes/` — Express route handlers
- `artifacts/api-server/src/lib/auth.ts` — simple token-based auth (SHA-256)
- `artifacts/keyvera/src/` — React frontend
- `attached_assets/logo_1778728053895.png` — KEYVERA logo (black/gold)

## Architecture decisions

- Token auth is Base64-encoded userId:role:timestamp stored in localStorage as `keyvera_token`
- Media stored as URLs in `media` table linked to `property`/`project` by entityType + entityId
- Properties require approval (`approved=true`) from Owner/CEO before appearing in featured lists
- Staff can create properties but not approve them; Owner/CEO auto-approve on create
- Bilingual content: Arabic and English fields stored separately (titleAr/titleEn, etc.)

## Product

- Public site: Home (hero, featured properties, projects, stats, testimonials), Properties listing with advanced filters, Property detail, Projects, About (leadership profiles), Contact
- Admin panel: Dashboard with stats and charts, Property/Project/Inquiry/User management
- Role-based access: Owner (full), CEO (executive), Staff (create/edit, pending approval)
- WhatsApp floating button on all pages (01206369219)
- RTL/LTR layout toggle for Arabic/English

## User preferences

- Company: KEYVERA Real Estate Brokerage, Alexandria, Egypt
- Owner: Engineer Nadeem Mahmoud Hassan (المهندس نديم محمود حسان)
- CEO: Engineer Doaa Edress (المهندسة دعاء إدريس)
- WhatsApp: 01206369219
- Brand colors: #000000 (black), #D4AF37 (gold), #1A1A1A (dark), #F8F8F8 (white)
- Typography: Cairo (Arabic) + Poppins (English)

## Gotchas

- Always run codegen after OpenAPI spec changes: `pnpm --filter @workspace/api-spec run codegen`
- Auth token is simple SHA-256 + Base64 — not production-grade JWT; suitable for migration to Firebase Auth
- `amenities` field in properties is a Postgres text array
- Numeric fields (price, floorArea, lat, lng) stored as `numeric` in DB, converted to `Number()` in routes

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
