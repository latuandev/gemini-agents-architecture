---
name: docker-runtime
description: Modify or troubleshoot the frontend Docker/Docker Compose and Next.js production runtime, including standalone output, ports, environment variables and startup.
---

# Docker and runtime workflow

The frontend is under `frontend/` and normally runs at `http://localhost:3000`.

## Preserve deployment model

- Multi-stage Dockerfile.
- Dependencies installed in a dependency stage.
- Next.js built in a builder stage.
- Runner uses Next.js standalone output.
- Runtime runs as a non-root user.
- Do not replace standalone output with a full source/node_modules production image unless explicitly required.

## Dependencies

When `package.json` changes, update `package-lock.json` with npm and keep it committed. Do not hand-edit integrity/resolution data. Prefer `npm ci` in reproducible build paths.

## Environment

Never commit secrets. Document required values in `.env.example` with safe placeholders. Keep internal app port `3000` unless explicitly changed.

## Verification

```bash
cd frontend
npm ci
npm run build
```

For container changes:

```bash
cd frontend
docker compose build
docker compose up -d
curl -f http://localhost:3000/
docker compose down
```

Do not claim Docker/network checks passed if unavailable.
