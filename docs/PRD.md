# Product Requirements Document (MVP)

This PRD is the **single source of truth for MVP scope** (ADR register D6). PRODUCT_BRIEF.md explains the why; where they differ, this document wins.

**MVP scope summary:** onboarding with editable targets; photo, text and manual meal logging; editable estimates with provenance and ranges; diary and daily dashboard; meal memory (Meal Context Engine v0, see `MEAL_CONTEXT_ENGINE.md`); weekly reflection; export and deletion. **Post-MVP:** weight trend, barcode scanning at scale, wearables and health integrations, additional languages.

## 1. Goals
- Ship a stable Android release from one shared codebase, followed by iOS (ADR 0003).
- Let an adult user log a meal through photo or text and review an editable estimate.
- Provide a useful daily nutrition summary.
- Learn repeat meals only from user-confirmed data.
- Establish privacy, safety and analytics foundations before growth.

## 2. Primary user stories
- As a new user, I can set a goal and receive a transparent starting target.
- As a user, I can photograph a meal and receive food candidates and a nutrition estimate.
- As a user, I can correct ingredients and portion sizes before saving.
- As a user, I can see which values came from a database, label, my input or AI estimate.
- As a user, I can quickly log a meal I eat often.
- As a user, I can review my day without being shamed for going over a target.
- As a user, I can export or delete my data.

## 3. Functional requirements

### Onboarding
- Explain purpose and limitations before collecting personal data.
- Require confirmation of adult eligibility for initial release.
- Support metric and imperial units.
- Goals: maintain, gradual weight loss, gradual weight gain, or general awareness.
- Make body metrics optional where possible.
- Show target calculation assumptions and allow edits.
- Do not create aggressive or medically prescriptive targets.

### Meal capture and analysis
- Support camera image, text description and manual entry (food search with quantity).
- Compress and upload image securely; enforce size/type limits.
- Backend sends image and relevant context to the configured vision model.
- Require schema-constrained output:
  - food candidates and normalized names;
  - ingredients where inferable;
  - preparation method assumptions;
  - portion estimate/range and confidence;
  - nutrient estimate per item;
  - clarification question, if required;
  - provenance and limitations.
- AI must not claim visual certainty about hidden oil, weight or ingredients.
- User can accept, edit or discard.
- Recalculate totals deterministically after edits.

### Diary and dashboard
- Group meals by local date and meal category.
- Display calories and macros with accessible visualizations.
- Clearly distinguish consumed, target and remaining.
- Avoid punitive red states or guilt language.
- Handle empty days, time-zone changes, offline mode and sync conflicts.

### Meal memory (Meal Context Engine v0)
See `MEAL_CONTEXT_ENGINE.md` for the full specification and success metrics.
- Save only after user confirmation.
- Store normalized meal composition and user's last confirmed portion.
- Offer "same as last time" and editable scaling.
- User can inspect, edit and delete memories.

### Insights
- Weekly summary based on logged data coverage.
- Explicitly indicate missing days and incomplete logging.
- No causal claims ("this food caused weight loss").
- Suggest one low-risk, practical next step.

### Account and privacy
- Authentication options: Google and email one-time code on Android; Sign in with Apple added for iOS (ADR 0007).
- Account deletion and export.
- Consent and photo-retention controls.
- Privacy policy and terms before public launch.

## 4. Non-functional requirements
- TypeScript strict mode.
- Crash reporting and structured, privacy-safe logs.
- API rate limits, timeouts, retries and idempotency.
- RLS for user-owned data.
- Accessibility: screen-reader labels, dynamic type, contrast, reduced motion.
- Performance: responsive dashboard and cached diary reads.
- Offline: read recent diary and queue safe user edits for sync.
- Localization-ready from day one (English first; architecture supports additional languages).

## 5. Acceptance criteria for first vertical slice
1. User can create account and complete onboarding.
2. User can submit a text-described meal.
3. Server returns schema-valid food items and estimates.
4. User can edit portion and food identity.
5. Daily totals update from deterministic nutrient records.
5a. User can save the meal as a usual meal and log it again with "same as last time" (D8).
6. Meal persists and reloads.
7. RLS prevents another user from reading the record.
8. Unit/integration tests cover calculation, validation and ownership.
9. Loading, error, retry and empty states are implemented.
10. No model secret or service-role key exists in the mobile bundle.
