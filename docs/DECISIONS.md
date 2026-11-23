# Decision register

All MVP planning decisions, accepted 2026-11-23. Material architecture decisions have ADRs in `docs/decisions/`.

| # | Decision | Outcome | Record |
| --- | --- | --- | --- |
| D1 | Repository visibility | Public, intentionally. Strict secret management: never commit credentials, keys or user data; GitHub secret scanning, push protection and a pre-commit secret check | This file |
| D2 | Platform sequence | Android-first from one Expo codebase; develop on Windows; Google Play first; iOS via EAS until a Mac and iPhone arrive (about two months), then iOS enablement milestone | ADR 0003 |
| D3 | Backend runtime and cost | Supabase free plan + Edge Functions (Deno) after a 1-day spike; upgrade only when usage passes 70% of a limit or revenue covers it; encrypted weekly backups and static pages on the founder's VPS | ADR 0002 |
| D4 | Target market | Global; English first; i18n-ready; international cuisines; metric default; beta testers across food cultures; no claim of comprehensive coverage, benchmark per cuisine | This file, ADR 0005 |
| D5 | Sequencing | Phase 0 discovery runs in parallel with Phase 1 foundation; Phase 2 is gated on Phase 0 findings | ROADMAP.md |
| D6 | MVP scope | PRD.md is the single source of truth; weight trend moved post-MVP; manual entry kept | PRD.md |
| D7 | Meal Context Engine v0 | Deterministic, user-confirmed memory, suggestions, corrections as AI context; success metrics defined | MEAL_CONTEXT_ENGINE.md |
| D8 | Meal memory timing | Moved into Phase 2 with text logging | ROADMAP.md |
| D9 | Data model | Estimate ranges, per-value provenance, recipes with yield factors, lean nutrient snapshots | ADR 0004 |
| D10 | Food data | USDA FDC imported; Open Food Facts live only; curated global dish library; regional sources later | ADR 0005 |
| D11 | AI provider | Chosen by benchmark on accuracy per cuisine, cost and latency; no-training terms in production; daily photo limit for free users | ADR 0006 |
| D12 | Tooling and product defaults | npm workspaces, Vitest, jest-expo, Maestro, i18next, Google + email OTP, Mifflin-St Jeor with safety floors, minimal offline scope | ADR 0007 |
