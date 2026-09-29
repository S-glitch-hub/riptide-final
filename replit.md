# Riptide Lifeguard Memory Agent

Riptide is a voice-first beach operations console that turns shift handoffs and incident notes into searchable memory for lifeguard teams.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Optional provider env: `GROQ_API_KEY`, `HINDSIGHT_API_URL`, `HINDSIGHT_API_KEY`, `HINDSIGHT_BANK_ID`, `BEACH_LAT`, `BEACH_LON`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/riptide/src/pages/operations.tsx` — live operations surface, memory capture, handoff, voice Q&A, forecast, and reflection UI.
- `artifacts/riptide/src/components/riptide-shell.tsx` — responsive app shell and zone navigation.
- `artifacts/riptide/src/pages/settings.tsx` — workspace and provider status view.
- `artifacts/api-server/src/routes/riptide.ts` — seeded memory, zone, forecast, handoff, incident, ask, and reflection routes.
- `lib/api-spec/openapi.yaml` — source-of-truth API contract; regenerate clients after changes.
- `artifacts/riptide/src/index.css` — Riptide visual tokens and motion utilities.

## Architecture decisions

- The app is usable without third-party credentials through a seeded demo memory store; weather uses Open-Meteo when available and falls back to deterministic safe demo data.
- The frontend talks to the shared `/api` service through generated OpenAPI React Query hooks, not hardcoded mock responses.
- Zone selectors use stable IDs in the UI while the API accepts both IDs and display names.
- Browser-native microphone permission and speech capabilities are progressive enhancements; typed input remains the reliable fallback.

## Product

- Live readiness and risk overview for the active beach zone.
- Incident memory capture with severity and searchable-memory responses.
- Incoming/outgoing shift handoff briefings.
- Voice-friendly question answering with clarification and memory citations.
- Forecast-aware safety insight and recurring pattern reflection.
- Workspace settings showing current zone, roster, and provider readiness.

## User preferences

- User requested a complete, polished, functional build from the supplied Riptide brief.

## Gotchas

- Restart `artifacts/api-server: API Server` after backend changes and `artifacts/riptide: web` after frontend/toolchain changes.
- The API server is mounted at `/api`; frontend calls should remain relative so preview and publish routing work.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
