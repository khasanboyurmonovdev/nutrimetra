# Architecture

## Recommended stack

| Layer | Choice | Reason |
|---|---|---|
| Mobile | Expo + React Native + TypeScript | Shared iOS/Android app, fast iteration, native modules when required |
| Navigation | Expo Router | File-based navigation and deep-link support |
| UI | NativeWind or Tamagui only after a small design-system spike; start with React Native StyleSheet + tokens | Avoid locking into a styling abstraction prematurely |
| Forms/validation | React Hook Form + Zod | Typed forms and runtime validation |
| Server state | TanStack Query | Caching, retries, mutations and offline-friendly patterns |
| Local state | Zustand (limited use) | Lightweight UI state; server data stays in Query |
| Backend | Supabase (Postgres, Auth, Storage, RLS, Edge Functions) | Fast managed foundation and relational data model |
| AI orchestration | Server-side TypeScript initially; isolate behind an `AIProvider` interface | Keep architecture simple and provider replaceable |
| Nutrition data | USDA FoodData Central + Open Food Facts where licensing/coverage fit; evaluate regional sources | Structured nutrient records rather than relying on LLM arithmetic |
| Billing | RevenueCat | Unified subscription entitlement management |
| Analytics | PostHog | Funnel, retention and feature analysis |
| Crash/error | Sentry | Mobile and backend diagnostics |
| Push | Expo Notifications initially | Cross-platform notification delivery |
| CI/CD | EAS Build/Submit + GitHub Actions | Reproducible mobile builds |
| Testing | Jest or Vitest for domain; React Native Testing Library; Maestro for E2E | Layered confidence |

### Why not Flutter/KMP/native first?
Flutter is a strong alternative, but the founder's TypeScript/React experience and the product's fast experimentation needs favor Expo/React Native. Kotlin Multiplatform can share domain logic but still requires more platform-specific UI work. Native Swift + Kotlin offers maximum platform control but doubles much of the product delivery effort. Revisit only if measured performance or platform capability requires it.

## Initial system shape
Mobile app → Supabase Auth → app API/Edge Function → AI provider + food data adapters → validated estimate → user confirmation → Postgres diary.

Keep the first backend modular, not microservice-based. Split AI inference into a separate Python service only if evaluation, model-serving, image pipelines or scaling justify it.

## Suggested monorepo
```text
nutrimetra/
  apps/
    mobile/
  packages/
    domain/          # pure calculations, entities, policies
    contracts/       # Zod schemas and API contracts
    design-system/   # tokens and shared primitives if needed
  supabase/
    migrations/
    functions/
  docs/
  .claude/
  CLAUDE.md
```

## Domain boundaries
- Identity & Consent
- User Profile & Goals
- Food Catalog
- Meal Analysis
- Meal Diary
- Meal Memory
- Nutrition Calculations
- Insights
- Subscription & Entitlements
- Notifications

## Data model (initial)
- `profiles`: user-owned profile and units.
- `goals`: target settings with effective dates and calculation assumptions.
- `foods`: normalized catalog references and source identifiers.
- `food_nutrients`: nutrient values, units, source and version.
- `meal_entries`: user, local date/time, meal type, notes, analysis status.
- `meal_items`: food reference, quantity, unit, preparation, nutrient snapshot, provenance.
- `meal_analysis_runs`: provider/model/version, input hash, structured result, confidence and cost metadata; avoid retaining raw image longer than necessary.
- `meal_memories`: user-confirmed reusable meals and portions.
- `consents`: purpose, policy version, timestamp and revocation.
- `subscriptions`: entitlement mirror; RevenueCat remains billing authority.

Use UUIDs, timestamps, explicit units, indexes on user/date, and RLS. Store nutrient snapshots with each logged item so historical diary totals do not silently change when catalog values update.

## AI estimation pipeline
1. Validate auth, request size and consent.
2. Store image in private temporary storage with short-lived access.
3. Run image safety and quality checks.
4. Vision model proposes foods, ingredients and preparation assumptions.
5. Resolve candidates against food database.
6. Deterministic nutrition engine computes nutrient totals from chosen records and portions.
7. Estimate uncertainty and decide whether clarification is needed.
8. Return editable draft to client; do not save as confirmed until user accepts.
9. Delete temporary image according to retention policy.
10. Record privacy-safe evaluation metadata.

## Accuracy and evaluation
- Build a benchmark set with varied cuisines, mixed dishes, lighting, angles and portion sizes.
- Have qualified reviewers annotate reference ingredients/weights where possible.
- Track food identification accuracy, portion error, calorie absolute error and user correction rates separately.
- Evaluate by food category and cuisine; do not report a single accuracy percentage without methodology.
- Never infer exact mass from image pixels alone.

## Offline and sync
- Cache recent diary and food memories locally.
- Queue user-authored edits with idempotency keys.
- Do not queue AI analysis as if it succeeded offline.
- Resolve concurrent edits explicitly; never silently overwrite confirmed user changes.

## Security
- RLS on every user-owned table.
- Supabase service-role key server-side only.
- Signed URLs for private image objects.
- Rate-limit analysis and authentication endpoints.
- Data minimization, export and deletion workflows.
- Redact personal/health data from logs and analytics.
