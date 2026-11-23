# Development Roadmap

This roadmap is milestone-based. Calendar estimates are planning assumptions for a small founder-led team and must be revised after discovery.

## Platform sequence (ADR 0003)

NutriMetra is one shared Expo + React Native codebase for Android and iOS. Delivery is **Android-first**:

1. Develop on Windows (Cursor, PowerShell, npm) with the Android emulator and a physical Android device.
2. First production release is on **Google Play**.
3. Until a Mac is available, iOS is verified only through **EAS cloud builds** (compile checks, and device builds if an iPhone and Apple Developer membership are available).
4. When a Mac is available (MacBook and iPhone planned within about two months, by late January 2027, before the Android release), run the **iOS enablement** milestone: Xcode, iOS Simulator, native debugging and physical iPhone testing.
5. Android-first is a *sequencing* choice, not an architecture choice. Nothing built for Android may block or complicate iOS later (see platform-neutrality rules in ADR 0003).

### Temporary limitations of Windows-only iOS development
- No Xcode, iOS Simulator, Instruments or native iOS debugging.
- No local iOS builds (`npx expo run:ios`, local prebuild compile). iOS builds run only on EAS.
- An EAS **simulator** build compiles iOS and needs no Apple Developer membership, but cannot be run without a Mac. It is a compile check only.
- Running a build on a physical iPhone needs a paid Apple Developer membership (device registration and signing via EAS) and an iPhone.
- iOS-only behaviour (permission prompts, HEIC photos, keyboard and safe-area layout, back-swipe navigation, Sign in with Apple, push via APNs) is untested until device or Mac testing starts. Treat these as known unverified risks, not as working.

## Phase 0 — Validate the wedge (1–2 weeks)
- Interview 15–20 target users, including people who abandoned calorie trackers, across different food cultures.
- Test three clickable prototypes: photo-first, meal-memory-first, and contextual clarification.
- Run a manual concierge test with 30–50 meals across multiple cuisines.
- Define a benchmark dataset and nutrition review process, with results reported per cuisine.
- Verify brand, domains, app-store naming, trademark and privacy requirements.
**Exit:** users understand the value proposition; a measurable pain point is validated; initial estimate quality is acceptable for a limited beta.

## Phase 1 — Foundation (1–2 weeks)
- Create Expo app (development builds, not Expo Go), strict TypeScript, lint, formatting and CI.
- Use Continuous Native Generation: `android/` and `ios/` are generated from `app.config.ts` and config plugins and are not hand-edited.
- Configure iOS fields in `app.config.ts` from day one (bundle identifier, permission usage strings), even though iOS ships later.
- Configure Supabase dev/staging projects, migrations and RLS.
- Set design tokens, navigation, auth and error reporting.
- Enable GitHub secret scanning and push protection; add a pre-commit secret check.
- Add architecture decision records.

**Exit criteria:**
1. Reproducible development build runs on the Android emulator and on a physical Android device, built from a clean clone by following the README.
2. An EAS iOS simulator build of the same commit compiles successfully (compile check; not run).
3. CI runs typecheck, lint and unit tests on every pull request and passes.
4. No secrets in the repository; secret scanning and push protection enabled.
5. Android and iOS app identifiers, app name and permission strings are set in `app.config.ts`.

## Phase 2 — First vertical slice (2–3 weeks)
- Onboarding and editable goals.
- Text meal logging.
- Food search/data adapter.
- Deterministic macro/calorie calculations.
- Diary and daily summary.
- Meal memory v0: save usual meal, "same as last time", scaling and context suggestions (`MEAL_CONTEXT_ENGINE.md`).
- Begin curated global dish library (ADR 0005).
**Exit:** complete log-to-dashboard flow and one-tap repeat logging on Android with tests and RLS; EAS iOS simulator build still compiles.

## Phase 3 — AI meal capture (2–3 weeks)
- Photo upload and secure temporary storage. Normalize images to JPEG so iOS HEIC photos work later.
- Structured vision analysis and food candidate resolution.
- Confidence/uncertainty representation.
- Clarification UX and user correction.
- Meal Context Engine: match analyses to memories, send corrections as context, reuse clarification answers.
- Run the AI provider evaluation and pick a provider (ADR 0006).
- Cost/latency instrumentation, daily photo-analysis limit and fallback behavior.
**Exit:** evaluated on benchmark set; estimates are clearly labeled and always editable; verified on Android; EAS iOS simulator build still compiles.

## Phase 4 — Retention differentiators (2 weeks)
- Measure Meal Context Engine metrics and refine ranking.
- Weekly reflection with data-coverage disclosure.
- Smart suggestions limited to safe, practical behavior.
- Push reminders that are opt-in and non-shaming.
**Exit:** usability test shows repeat logging is faster and users trust the results.

## Phase 5 — Android closed beta (2–4 weeks)
- Google Play internal testing, then closed testing with 50–100 invited adult testers from different food cultures.
- If the Play account is subject to the closed-testing requirement for new personal accounts, run that test long enough to unlock production access.
- Crash-free sessions, funnel and retention instrumentation.
- Interview users who stop logging after day 1/3/7.
- Tune AI cost, rate limits, abuse controls and subscription packaging.
**Exit:** stable Android release candidate and evidence of repeat use.

## Phase 6 — Google Play production launch
- Store assets, Data safety form, health app declarations, privacy policy, terms, support and deletion workflows.
- RevenueCat subscriptions after value is proven.
- ASO and creator-led organic acquisition.
- Monitor retention, refund rate, reviews, estimate corrections and unit economics.

## iOS track — Mac-based iOS enablement (starts when a Mac is available; 2–3 weeks)
Runs in parallel with whichever Android phase is active when the Mac arrives. It must not fork the codebase.
- Set up Xcode, iOS Simulator and a physical iPhone development build.
- Run the full test suite and Maestro flows on iOS; fix platform gaps in shared code.
- Verify iOS-specific behaviour: camera and photo-library permissions, HEIC handling, safe areas, keyboard, back-swipe navigation, dynamic type, VoiceOver.
- Add Sign in with Apple (required on iOS when Google sign-in is offered), APNs push, and the privacy manifest.
- Set up App Store Connect, TestFlight, App Privacy labels and subscription products in RevenueCat.
**Exit:** iOS reaches parity with the current Android milestone; TestFlight beta running.

Then: iOS TestFlight beta → App Store review → App Store launch.

## First 10 engineering tickets
1. Initialize Expo + TypeScript + Expo Router with development builds and CNG (Android and iOS config).
2. Add environment configuration and secret-handling rules (split client and server env files; secret scanning).
3. Configure Supabase auth and local development.
4. Create migrations for profiles, goals and diary entities.
5. Implement RLS policies and tests.
6. Build domain nutrition types and deterministic calculation functions.
7. Implement text meal logging endpoint with Zod contracts.
8. Build diary/dashboard screens and loading/error/empty states.
9. Add food data provider interface and first provider adapter.
10. Add photo-analysis draft flow behind a feature flag.

Plus, at every milestone: confirm the EAS iOS simulator build compiles.

## Release gates
- No public release until privacy policy, terms, deletion and data disclosures are complete.
- No nutrition accuracy claims without a documented evaluation methodology, reported per cuisine.
- No claim of comprehensive global food coverage; limitations are stated in the app.
- No subscription paywall before the free core loop demonstrates repeat value.
- No broad health/wearable permissions until a user-facing feature requires them.
- No iOS release until the Mac-based iOS enablement milestone exit is met.
