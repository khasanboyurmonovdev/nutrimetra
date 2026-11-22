# Development Roadmap

This roadmap is milestone-based. Calendar estimates are planning assumptions for a small founder-led team and must be revised after discovery.

## Phase 0 — Validate the wedge (1–2 weeks)
- Interview 15–20 target users, including people who abandoned calorie trackers.
- Test three clickable prototypes: photo-first, meal-memory-first, and contextual clarification.
- Run a manual concierge test with 30–50 meals across multiple cuisines.
- Define a benchmark dataset and nutrition review process.
- Verify brand, domains, app-store naming, trademark and privacy requirements.
**Exit:** users understand the value proposition; a measurable pain point is validated; initial estimate quality is acceptable for a limited beta.

## Phase 1 — Foundation (1 week)
- Create Expo app, strict TypeScript, lint, formatting and CI.
- Configure Supabase dev/staging projects, migrations and RLS.
- Set design tokens, navigation, auth and error reporting.
- Add architecture decision records.
**Exit:** reproducible development build on iOS simulator and Android emulator/device.

## Phase 2 — First vertical slice (2 weeks)
- Onboarding and editable goals.
- Text meal logging.
- Food search/data adapter.
- Deterministic macro/calorie calculations.
- Diary and daily summary.
**Exit:** complete log-to-dashboard flow with tests and RLS.

## Phase 3 — AI meal capture (2–3 weeks)
- Photo upload and secure temporary storage.
- Structured vision analysis and food candidate resolution.
- Confidence/uncertainty representation.
- Clarification UX and user correction.
- Cost/latency instrumentation and fallback behavior.
**Exit:** evaluated on benchmark set; estimates are clearly labeled and always editable.

## Phase 4 — Retention differentiators (2 weeks)
- Confirmed meal memory and one-tap repeat.
- Weekly reflection with data-coverage disclosure.
- Smart suggestions limited to safe, practical behavior.
- Push reminders that are opt-in and non-shaming.
**Exit:** usability test shows repeat logging is faster and users trust the results.

## Phase 5 — Closed beta (2–4 weeks)
- 50–100 invited adult testers.
- Crash-free sessions, funnel and retention instrumentation.
- Interview users who stop logging after day 1/3/7.
- Tune AI cost, rate limits, abuse controls and subscription packaging.
**Exit:** stable release candidate and evidence of repeat use.

## Phase 6 — Public launch
- Store assets, privacy disclosures, terms, support and deletion workflows.
- RevenueCat subscriptions after value is proven.
- ASO and creator-led organic acquisition.
- Monitor retention, refund rate, reviews, estimate corrections and unit economics.

## First 10 engineering tickets
1. Initialize Expo + TypeScript + Expo Router.
2. Add environment configuration and secret-handling rules.
3. Configure Supabase auth and local development.
4. Create migrations for profiles, goals and diary entities.
5. Implement RLS policies and tests.
6. Build domain nutrition types and deterministic calculation functions.
7. Implement text meal logging endpoint with Zod contracts.
8. Build diary/dashboard screens and loading/error/empty states.
9. Add food data provider interface and first provider adapter.
10. Add photo-analysis draft flow behind a feature flag.

## Release gates
- No public release until privacy policy, terms, deletion and data disclosures are complete.
- No nutrition accuracy claims without a documented evaluation methodology.
- No subscription paywall before the free core loop demonstrates repeat value.
- No broad health/wearable permissions until a user-facing feature requires them.
