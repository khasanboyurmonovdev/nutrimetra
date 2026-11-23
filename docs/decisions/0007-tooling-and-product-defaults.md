# ADR 0007: Tooling and product defaults

- **Status:** Accepted (2026-11-23)
- **Context:** Several small choices were left open. Each is reversible; recording them stops them being re-debated.

## Decision

| Area | Choice | Reason |
| --- | --- | --- |
| Package manager / monorepo | npm workspaces | Founder uses npm on Windows; Expo supports npm monorepos; no extra tool |
| Unit tests (`packages/*`) | Vitest | Fast, ESM-native, runs domain code without React Native |
| App tests | jest-expo + React Native Testing Library | Expo's supported preset |
| E2E | Maestro (from Phase 4) | Same flows run on Android and iOS |
| Lint / format | ESLint (flat config) + Prettier | Standard; enforced in CI |
| i18n | i18next + react-i18next + expo-localization | English first; all user-facing strings externalized from day one |
| Units | Metric default; imperial option stored per profile | D4 |
| Auth | Google sign-in + email one-time code (OTP) on Android; Sign in with Apple added in the iOS milestone | OTP avoids mobile magic-link deep-link failures; Apple required on iOS when Google is offered |
| Identity linking | Supabase user id is the stable id; RevenueCat app user id = Supabase user id | Accounts and subscriptions follow the user across platforms |
| Energy target | Mifflin-St Jeor BMR × activity factor; weight change capped at 0.5% of body weight per week; never below 1,200 kcal (women) / 1,500 kcal (men) or BMR, whichever is higher (higher floor if sex is not provided); users can edit with a non-shaming warning | Conservative, transparent, not medically prescriptive |
| Offline (MVP) | Persisted TanStack Query cache for recent diary and memories; offline queue only for manual entries and "same as last time" logs, with idempotency keys; AI analysis requires network | Smallest scope that meets the PRD |
| Analytics | PostHog free tier, EU or US cloud chosen once; no health values in events; session replay off | Privacy by design |
| Errors | Sentry free tier with PII scrubbing | |
| Builds | EAS free tier; local Android builds when quota runs short | Budget |
| App identifiers | `com.nutrimetra.app` for Android package and iOS bundle id, after a basic trademark check | Hard to change after store submission |

## Consequences
All are cheap to change before launch except the app identifiers.
