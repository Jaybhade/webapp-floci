# Frontend project status

Last updated: 2026-09-27. This records verified milestones and decisions; it is not a claim that the application is production ready.

## Goal and decisions

- Build an AI knowledge assistant with a Next.js frontend and a separate Express/TypeScript backend.
- The user implements application changes; the assistant teaches from first principles, gives small exercises, and verifies results unless explicitly asked to implement changes.
- Deploy the existing small application through Floci local compute before building the full MVP. No real AWS account is part of the current plan.
- Do not modify Floci's source or configuration. Provision application resources through cloud-facing interfaces.
- Use Node 25 by explicit user choice. Frontend uses pnpm and commits `pnpm-lock.yaml`; backend uses npm and commits `package-lock.json`.
- Use a hosted DeepSeek model from the backend later. The user calls it DeepSeek v4.1; provider details and model identifier still need verification. Never expose the API key to the frontend.
- Defer writing an automated test suite until after the MVP, while continuing build and functional verification.

## Repositories

- Frontend: https://github.com/Jaybhade/webapp-floci
- Backend: https://github.com/Jaybhade/backend-floci
- Both local `main` commits matched GitHub when last checked. Preserve the existing Next.js guidance in `AGENTS.md`.

## Completed and verified

- Next.js starter application exists, with Next.js 16.3.6, React, and TypeScript.
- Lint and production build, including TypeScript checking, passed earlier through npm scripts. At that time, pnpm could not run under the shell's Node 22.12; Node 25 is the agreed project runtime.
- Frontend still displays the starter page. There is no frontend-to-backend flow, frontend Docker image, or Floci frontend deployment yet.
- Backend has migrated to Express and TypeScript. Its compiled app and migrated standalone Docker container passed route checks.
- Backend has a separate application Compose configuration; Floci remains independently managed.
- AWS CLI installation is reported complete and the executable is present. The Floci CLI profile and API connectivity have not been verified.

## Shared measured traffic baseline

The standalone Express backend Docker container handled 10 requests/second to `/health` for 30 seconds: 300 successful requests, zero errors, median 3.30 ms, p95 10.93 ms, and p99 18.01 ms. It was limited to one CPU and 256 MiB memory on the user's Apple M1 Pro laptop.

These are backend health-endpoint results only. Frontend serving capacity, end-to-end capacity, and Floci EC2 capacity remain unmeasured. Do not extrapolate this result to database or AI workloads or real AWS performance.

## Next milestone

1. Complete AWS CLI connectivity to Floci and provision the first application instance.
2. Deploy the existing backend, then deploy the existing Next.js application.
3. Connect the deployed frontend to `/api/info`, verify the browser flow, and document the update and rollback procedure.
4. Continue building and deploying features incrementally. Add CI/CD once manual deployment is repeatable.

## Deliberately deferred

- Automated test suite, GitHub Actions, Terraform, structured application logging, and a formal rollback mechanism are not implemented yet. Address deployment logging and repeatable updates during the first deployment.
- Notes, authentication, persistence, AI generation, uploads, RAG, workers, and scaling remain future milestones.
