---
name: change-verification
description: Verify meaningful frontend code changes without expanding scope. Use after runtime modifications when a regression is plausible or validation is requested.
---

# Change verification

Validate what changed; do not turn verification into unrelated cleanup.

## Baseline

```bash
cd frontend
npm run build
```

Use `npm ci` first when dependencies are absent or clean-install reproduction matters. Do not add a test/lint framework solely to verify one feature unless requested.

## Runtime

When routing, rendering, interaction or Docker startup changes and the environment supports it, run the app and verify affected routes. For Docker-specific work, follow `/docker-runtime`.

## Diff review

Confirm only intended files/behavior changed; check for accidental formatting churn, secrets/local environment values, responsive regressions and accessibility regressions.

Report exactly which checks passed and any unavailable boundary.
