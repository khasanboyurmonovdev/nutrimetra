# ADR 0002: Initial backend

- **Status:** Accepted (2026-11-23), pending a one-day validation spike
- **Decision:** Supabase Postgres/Auth/Storage/RLS with Supabase Edge Functions (Deno, TypeScript) for server-side logic, on the Supabase **free plan** until usage or revenue justifies upgrading.
- **Rationale:** Relational meal/nutrient data, fast setup, integrated auth/storage and strong ownership controls, with no separate server to host or secure and no cost before launch.

## Validation spike (before Phase 1 app work)
- Shared `packages/domain` and `packages/contracts` (Zod) import cleanly in both Metro (React Native) and Deno.
- A real vision-model call completes within Edge Function limits. Waiting on network I/O does not count toward the CPU-time limit, so images are resized on the device, not in the function.
If the spike fails, fall back to a small Node service behind the same contracts.

## Environments and cost policy
- Two free Supabase projects: `dev` and `beta/production`. Staging runs locally with the Supabase CLI (Docker on WSL2).
- Keep the free plan workable: lean schema (core nutrients as typed columns), delete meal photos right after analysis, cache food search on the device, usage alerts at 70% of each limit.
- The free plan has no backups: run a weekly encrypted `pg_dump` to the founder's existing VPS. Backups never go into the public GitHub repository.
- **Upgrade trigger:** any limit above 70% of quota, or subscription revenue that covers the paid plan. Not a fixed user count. Expected first constraint: 500 MB database size, at a few thousand active users.

## VPS use
The existing VPS (which also hosts another product) is used only for encrypted backups and static pages (privacy policy, account-deletion page required by Google Play). It does not host the NutriMetra database or API, to keep the two products' data isolated. NutriMetra runs under a separate Linux user or container with separate secrets.

## Consequences
- Keep privileged operations server-side; use migrations and RLS tests. Avoid microservices initially.
- Edge Functions run on Deno, not Node; keep business logic in shared packages so a Node port only means rewriting thin handlers.
- No background jobs beyond what Supabase offers (pg_cron, queues) in MVP.

## Revisit when
AI workloads, background jobs or operational requirements outgrow managed functions, or the free-plan limits are reached; then upgrade the plan or extract a dedicated service behind stable contracts.
