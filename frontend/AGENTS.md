# Frontend AGENTS.md

## Project overview

These instructions apply to `frontend/`, a Next.js App Router application using Next.js 15, React 19, TypeScript, plain CSS, `next/image`, and Docker/Docker Compose.

- `frontend/app/` contains route-level UI and global styles.
- `frontend/components/` contains reusable React components.
- `frontend/data/` is available for static presentation data or temporary typed mock data when needed.
- `frontend/hooks/` contains reusable React hooks.
- `frontend/lib/` contains application/library utilities and API clients.

## Scope and working principles

- Follow root `AGENTS.md`; these rules take precedence under `frontend/`.
- Do not change backend code during a frontend-only task unless explicitly requested.
- Implement requested features completely within scope and preserve existing behavior unless explicitly changed.
- Prefer small, reviewable diffs and reuse existing components/conventions before adding abstractions.
- Do not introduce Tailwind, a component framework, state-management library, or another dependency unless materially required.
- Do not rewrite/reformat unrelated files solely for style consistency.
- Never commit secrets; document new variables in `.env.example` using safe placeholders.

## Frontend architecture

- Prefer Server Components by default.
- Add `'use client'` only for browser state, effects, event-driven interaction or browser-only APIs.
- Keep route-level UI in `app/**/page.tsx` and reusable UI in `components/`.
- Application source is TypeScript-first; do not introduce `.js`, `.jsx`, or `.mjs` application source files.
- Use `next/link` for internal route navigation.
- Use `next/image` for application imagery where appropriate. Update `next.config.ts` intentionally when new remote hosts are required.
- Keep global styling in `app/globals.css` unless a feature clearly benefits from a more local structure. Do not reformat the whole stylesheet for a local change.

## TypeScript

- Use `.ts` for non-JSX files and `.tsx` for JSX.
- Prefer explicit domain types/interfaces for shared data models and component props.
- Prefer inference for simple local values.
- Avoid `any`; use it only when no reasonable typed alternative exists.
- Do not weaken existing types merely to make an implementation compile.
- Preserve repository structure and architectural patterns unless a structural refactor is explicitly requested.

## Data and API boundaries

- Keep source/domain data truthful; do not falsify source values merely to satisfy presentation needs.
- Keep presentation-only state/properties separate from source/domain metadata.
- Centralize reusable API/client logic in `lib/` rather than duplicating fetch logic across route components.
- Keep API contracts explicit and typed.
- Treat backend validation/authorization as authoritative; client validation is for UX and must not replace server enforcement.
- Never store secrets or server session credentials in browser storage.

## Responsive and accessibility expectations

- Design for desktop and mobile behavior when changing layouts.
- Interactive controls must be keyboard reachable and have accessible labels when visible text is insufficient.
- Preserve visible focus states.
- Do not rely on hover as the only way to expose an action required on touch devices.
- Prefer semantic HTML before adding ARIA.

## Docker and local runtime

Primary commands:

```bash
cd frontend
npm ci
npm run dev
npm run build
```

Docker workflow:

```bash
cd frontend
docker compose up --build
```

Expected local URL: `http://localhost:3000`.

The production image uses Next.js standalone output. Preserve that model unless deployment strategy is explicitly changed.

## Skill usage

Use repository skills only when they materially match the task:

- `/frontend-feature` for pages, components, interactions or frontend feature work.
- `/docker-runtime` for Docker/Compose, Next.js runtime, ports or environment work.
- `/change-verification` after meaningful runtime changes when regression risk exists or validation is requested.

## Validation

For frontend runtime changes, `npm run build` is the baseline production check when dependencies are available. Do not add a lint/test framework solely to validate one unrelated change. If service credentials, network access or runtime dependencies prevent full verification, finish the locally verifiable implementation and report the exact boundary.
