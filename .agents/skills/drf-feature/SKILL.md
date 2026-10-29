---
name: drf-feature
description: Implement or extend Django REST Framework backend features including domain models, migrations, serializers, services, ModelViewSets, routers, filtering, search, ordering, pagination, permissions, tasks, and admin integration.
---

# DRF feature workflow

Follow `backend/AGENTS.md` first.

## 1. Find the owning domain

Use or create the smallest appropriate `backend/apps/<domain>/` app with the canonical app layout. Do not create extra packages without a concrete responsibility.

## 2. Model data explicitly

Use PostgreSQL-compatible Django fields. Use repository `EnumChoices` for project-defined choices. Generate and inspect migrations. Add indexes/constraints only when the task or an obvious invariant justifies them.

## 3. Keep API layers focused

Models persist data and enforce model-level invariants; serializers validate/represent; views stay thin. Prefer `ModelViewSet`, routers, `django-filter`, `SearchFilter`, `OrderingFilter`, and pagination for standard resource APIs.

## 4. Use services only for workflows

Use `services/` for orchestration, transactions, external infrastructure, reusable workflows and state transitions. Do not create services solely to wrap trivial ORM calls. Services must not depend on HTTP request/response objects.

## 5. Errors and messages

Catchable domain errors belong in `apps/<domain>/exceptions.py`. User-facing API messages should use the repository message convention when centralized messages are in use. Never expose internal exception details.

## 6. Query efficiency

Use `select_related` / `prefetch_related` for obvious N+1 cases without prematurely optimizing unrelated queries.

## 7. Tests

Add focused tests for introduced behavior, invalid input and important failure paths. Cover filters/search/ordering/pagination/permissions introduced by the task.

## 8. Finish narrowly

Do not modify frontend code during a backend-only task unless explicitly requested. Use `/backend-verification` before reporting completion.
