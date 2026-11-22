# ADR 0002: Initial backend

- **Status:** Proposed
- **Decision:** Supabase Postgres/Auth/Storage/RLS with server-side TypeScript functions for the first release.
- **Rationale:** Relational meal/nutrient data, fast setup, integrated auth/storage and strong ownership controls.
- **Consequences:** Keep privileged operations server-side; use migrations and RLS tests. Avoid microservices initially.
- **Revisit when:** AI workloads, background jobs or operational requirements outgrow managed functions; then extract a dedicated service behind stable contracts.
