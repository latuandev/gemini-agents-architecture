---
name: backend-verification
description: Verify Python, Django, DRF, PostgreSQL, migrations, Gunicorn, Celery, Redis, and Docker backend changes without expanding task scope.
---

# Backend verification workflow

Follow `backend/AGENTS.md` and validate only affected surfaces.

## Python and Django checks

```bash
cd backend
python -m compileall apps common config
ruff check .
ruff format --check .
python manage.py check
python manage.py makemigrations --check
python manage.py test
```

If Ruff is not configured, do not add it solely to verify an unrelated feature; report those checks as unavailable.

## Migration review

For model changes, confirm the expected migration, inspect it for accidental schema churn, run `makemigrations --check`, and run relevant tests against PostgreSQL when supported.

## API checks

Verify relevant list/detail behavior, validation errors, filtering, search, ordering, pagination and permissions. Prefer Django/DRF tests over manual HTTP-only checking when tests can express behavior reliably.

## Docker/runtime checks

```bash
cd backend
docker compose config
docker compose build
docker compose up -d
```

Verify affected endpoints on `http://localhost:8000` when possible. Never claim Docker/PostgreSQL/Gunicorn/Celery was tested if the environment did not support it.

## Diff review

Confirm only requested behavior changed, no secrets/local-only values were committed, `.env.example` reflects new variables, migrations contain no unintended operations, and public API errors do not expose implementation details.

Report exactly which commands passed and which could not run.
