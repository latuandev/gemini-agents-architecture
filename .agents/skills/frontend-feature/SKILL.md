---
name: frontend-feature
description: Implement or extend a Next.js App Router frontend feature including pages, components, forms, client interactions, typed data and API integration.
---

# Frontend feature workflow

Follow `frontend/AGENTS.md` first.

## 1. Locate the smallest surface

Typical locations: `app/<route>/page.tsx`, `components/`, `data/`, `hooks/`, `lib/`, `app/globals.css`, and `next.config.ts` only when configuration must change. Do not refactor unrelated behavior.

## 2. Choose Server/Client boundary deliberately

Prefer Server Components. Use Client Components only for React state, effects, browser APIs, event-driven interaction or interactive form behavior that cannot stay server-rendered. Keep the client boundary as small as practical.

## 3. Reuse before adding

Extend an existing component cleanly when appropriate. Create a new reusable component when it has a clear responsibility or improves understandability. Do not add third-party dependencies for behavior React, Next.js or CSS can implement simply.

## 4. Keep data explicit and typed

Use clear objects rather than positional arrays for temporary data. Preserve source/domain truth and use separate presentation properties instead of mutating domain metadata for layout needs.

## 5. Style and accessibility

Add targeted styles; do not reformat whole stylesheets. Account for desktop/mobile, keyboard access, visible focus, semantic markup and touch-device behavior.

## 6. Finish completely

Implement the whole requested user flow rather than leaving dead controls or placeholder handlers unless a visual-only prototype is explicitly requested. Use `/change-verification` when appropriate.
