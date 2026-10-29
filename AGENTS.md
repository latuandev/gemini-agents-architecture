# AGENTS.md

## Repository scope

This repository contains two application areas with separate implementation conventions:

- `frontend/` — Next.js / React / TypeScript frontend.
- `backend/` — Django / Django REST Framework / PostgreSQL backend.

## Task routing

Before changing code, identify which application area owns the task.

### Frontend tasks

For work under `frontend/`, read and follow `frontend/AGENTS.md`. Frontend-specific instructions take precedence for files under `frontend/`. Relevant skills are discovered from `.agents/skills/<skill-name>/SKILL.md`. Use `/frontend-feature`, `/docker-runtime`, or `/change-verification` when relevant.

### Backend tasks

For work under `backend/`, read and follow `backend/AGENTS.md`. Backend-specific instructions take precedence for files under `backend/`. Relevant skills are discovered from `.agents/skills/<skill-name>/SKILL.md`. Use `/drf-feature` or `/backend-verification` when relevant.

### Cross-stack tasks

When a task explicitly changes both frontend and backend:

1. Read both scoped `AGENTS.md` files.
2. Preserve each side's conventions within its own directory.
3. Keep the API contract explicit between the two sides.
4. Do not refactor unrelated code in either application area.

## Repository-wide principles

- Implement only the requested scope.
- Prefer small, reviewable diffs over broad refactors.
- Reuse existing conventions before introducing new abstractions or dependencies.
- Do not rewrite or reformat unrelated files solely for style consistency.
- Never commit secrets. Document required environment variables with safe placeholders in the appropriate `.env.example`.
- If an applicable check cannot be executed, report that limitation explicitly instead of claiming it passed.

## Git workflow (GitFlow)

- Follow GitFlow branch naming where applicable: `feature/<lowercase-kebab-case>` for features and `bugfix/<lowercase-kebab-case>` for fixes.
- Use meaningful, descriptive pull request titles and commit messages. Capitalize the first letter and avoid decorative special characters.
- Do not create branches, commit, rebase, merge, or push unless explicitly requested by the user.
- Before any Git operation that alters history or remote state, explain its effect and request approval.

## Antigravity CLI operating instructions

- Root and directory-scoped `AGENTS.md` files are always-on constraints. Apply `frontend/AGENTS.md` to frontend files and `backend/AGENTS.md` to backend files. For cross-stack tasks follow both.
- Repository skills reside directly in `.agents/skills/<name>/SKILL.md` and are task-specific workflows, not replacements for these rules.
- Read the applicable skill when its task matches; optional direct invocation uses `/frontend-feature`, `/docker-runtime`, `/change-verification`, `/drf-feature`, or `/backend-verification`.
- Never claim that build, Docker, database, Git, or external checks passed unless actually executed and successful.
- Never install packages, alter API contracts, run migrations against non-disposable databases, or execute destructive commands without clear authorization.
