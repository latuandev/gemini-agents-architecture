# Backend AGENTS.md

## Scope

- Follow the root `AGENTS.md` for repository-wide routing and principles.
- These backend instructions take precedence for files under `backend/`.
- Do not change frontend code during a backend-only task unless explicitly requested.

## Stack

The backend uses Python, Django, Django REST Framework, PostgreSQL, Redis, Celery, Gunicorn, and Docker / Docker Compose. Do not replace these technologies unless the task explicitly requests it.

## Backend architecture

Keep the backend idiomatic Django/DRF and avoid speculative infrastructure.

```text
backend/
├── manage.py
├── config/
├── apps/
│   └── <domain>/
├── common/
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

New domain apps should use this canonical layout unless an existing app establishes a stricter precedent:

```text
apps/<domain>/
├── migrations/
├── services/
├── __init__.py
├── apps.py
├── models.py
├── serializers.py
├── tasks.py
├── tests.py
├── urls.py
└── views.py
```

Add domain-specific subpackages only when a clear responsibility justifies them.

## Domain boundaries

Domain-specific models, services, serializers, tasks, views, validation and infrastructure must remain inside their owning `apps/<domain>/` package unless genuinely reusable across multiple domains or governed by a repository-wide convention.

Shared code belongs in `common/` only when it is reusable across multiple domains or represents project-wide infrastructure. Do not move code into `common/` merely because multiple modules inside one domain use it.

Domain-specific seed data, when needed, belongs under `apps/<domain>/data/`. Keep seed data deterministic and domain-owned. Do not create runtime dependencies on frontend source files solely to load backend seed data.

## Domain exceptions and user-facing messages

Catchable domain exceptions belong in `apps/<domain>/exceptions.py`. Shared exceptions belong in `common/exceptions.py` only when genuinely cross-domain.

Human-readable messages returned through APIs should be centralized in `common/messages.py` when the project needs centralized/localized messages. Keep stable `lowercase_snake_case` keys and never expose internal exception details, filesystem paths, commands, credentials, stack traces, or implementation details.

Domain exceptions should represent deterministic error conditions independently from presentation text; the API-facing layer should map exceptions to safe messages.

## Django and DRF conventions

- Keep views thin.
- Keep serializers focused on request validation and response representation.
- Put application workflows and business orchestration in the owning domain's `services/` package.
- Use explicit database transactions for multi-step state changes when atomicity is required.
- Prefer explicit service orchestration over Django signals for core workflows.
- Enforce ownership or the appropriate permission policy for user-scoped resources.
- Never expose internal implementation details through API responses.
- Use `settings.AUTH_USER_MODEL` instead of importing Django's default `User` model directly.
- Generate and inspect Django migrations for model schema changes.
- Use DRF `ModelViewSet` and routers for standard resource CRUD unless a different endpoint type is justified.
- Prefer `django-filter`, DRF `SearchFilter`, and `OrderingFilter` over manual query-parameter parsing.
- Use pagination for list endpoints expected to grow.
- Use `select_related` / `prefetch_related` when serializer access would otherwise create an obvious N+1 query.
- Do not add custom actions when standard resource behavior or normal filtering/querying already solves the requirement.
- Keep Python model field names in `snake_case`; map camelCase API fields explicitly in serializers when required by the contract.

## Model choices

Do not use Django `TextChoices` or `IntegerChoices` for project-defined model choices. Use `common.utils.enum_choices.EnumChoices` and consume enums through `choices=YourEnum.choices()`.

Place project-wide choice enums in `common/constants.py` under the choice-enum section and non-choice constants under the constants section.

## Shared utilities

Reusable pure Python utilities shared across domains belong under `common/utils/`. Use focused modules for clear responsibilities rather than growing a generic helpers module indefinitely. Do not place domain business logic in `common/utils/`. Keep shared utilities independent of Django domain models whenever practical.

## Python style

Follow PEP 8 and repository formatter/linter configuration.

- 4 spaces for indentation.
- `snake_case` for variables, functions, methods, modules and packages.
- `PascalCase` for classes.
- `UPPER_SNAKE_CASE` for constants.
- English for source identifiers, comments, docstrings, configuration and project documentation.
- Imports: standard library, third-party packages, local project imports.
- Prefer explicit names, readable code, small focused functions, existing abstractions, and useful type hints.
- Avoid unnecessary abstractions, mutable default arguments, broad silent exception handling, and unrelated refactors.
- Do not use Python built-in names such as `str`, `list`, or `dict` as local variable names.

Sentence-like comments and human-readable strings should begin with an uppercase letter. Stable identifiers, keys, paths, commands and other non-sentence values are excluded.

## Docstrings

Project-owned public classes, service functions, reusable utilities and non-trivial methods must include concise English PEP 257 docstrings when they define meaningful behavior or contracts.

Use multi-line triple-quoted formatting even for short project-owned docstrings, with opening and closing quotes on their own lines. Document important arguments, returns, exceptions, side effects or transactional behavior when not obvious from the signature. Generated migrations and framework boilerplate may omit docstrings.

## Services

Use `services/` for workflows, multi-step state changes, transactions, external infrastructure, task dispatch and reusable domain orchestration. Services must not depend on HTTP request/response objects. Do not create a service merely to wrap one trivial ORM call.

## Celery

- Keep tasks as thin execution entry points and delegate application logic to services.
- Prefer task contracts based on stable identifiers.
- Do not pass credentials, large binary payloads, large serialized domain objects, or authoritative business state through task arguments.
- Assume tasks may be retried or delivered more than once; make task/domain execution retry-safe and idempotent where appropriate.
- Do not introduce another scheduler, broker, or queue system unless a concrete requirement needs it.

## Tests

New behavior must include appropriate focused tests for normal behavior, invalid input, important failure paths, state transitions, security-sensitive boundaries and API behavior when relevant. Do not weaken/remove existing tests merely to make new code pass. Promote `tests.py` to a `tests/` package when the suite becomes large enough to justify it.

## File naming

- Python modules/packages: `lowercase_snake_case`.
- Project-owned Markdown files: `lowercase-kebab-case.md`.
- ADR files: `adr-NNN-short-description.md`.
- Preserve ecosystem/tool-recognized filenames such as `AGENTS.md`, `README.md`, `.gitignore`, `.dockerignore`.

## PostgreSQL and migrations

- PostgreSQL is the application database for local and production runtime.
- Do not introduce SQLite as an alternate application database unless explicitly requested.
- Schema changes must use Django migrations.
- Inspect generated migrations before completion.
- Do not edit an already-applied migration merely to reshape history unless explicitly requested.

## Docker and Gunicorn

The backend must remain runnable through Docker Compose. Use Gunicorn as the application server, PostgreSQL as a separate service, Redis as a separate service, and Celery as a worker service when asynchronous work exists. Keep secrets/environment-specific configuration outside source code and maintain `.env.example`. Do not add extra infrastructure without a concrete requirement.

Expected development API origin: `http://localhost:8000`.

## Verification

Run applicable checks before completion:

```bash
cd backend
python -m compileall apps common config
ruff check .
ruff format --check .
python manage.py check
python manage.py makemigrations --check
python manage.py test
```

When Docker/runtime changes apply and Docker is available:

```bash
cd backend
docker compose config
docker compose build
docker compose up -d
```

If tooling is not configured or a check cannot run, report the limitation explicitly. Do not report completion while required available checks are failing.
