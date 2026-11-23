# Meal Context Engine — v0 specification

**Status:** Accepted for MVP (2026-11-23). This is a hypothesis to validate, not a proven advantage.

## Hypothesis
People eat a small set of recurring meals. If NutriMetra remembers what a user has *confirmed* (meals, portions, preparation, corrections), repeat logging becomes much faster and estimates need fewer edits over time. That should improve retention compared with photo-only trackers.

## Principles
- Learn **only from user-confirmed data**. A model guess is never stored as a fact.
- **Deterministic first.** v0 uses rules, retrieval and ranking. No model training, no embeddings, no personal ML models.
- Every suggestion is visible, editable and deletable by the user.
- Memory data is user-owned, covered by RLS, export and deletion.

## What v0 stores
| Table | Contents |
| --- | --- |
| `meal_memories` | User-confirmed reusable meal: name, cuisine tag (optional), usual meal type, times logged, last logged at |
| `meal_memory_items` | Food or recipe reference, last confirmed quantity and unit, preparation (e.g. fried, boiled), oil/fat assumption |
| `user_corrections` | When a user edits an AI estimate: original candidate, corrected food, original and corrected quantity, preparation change, timestamp |

A memory is created when the user taps "Save as usual meal", or offered after the same composition is confirmed 2 times.

## What v0 does
1. **Same as last time.** One tap logs a memory with its last confirmed portions. Users can scale (½×, 1×, 1½×, 2×) or edit before saving.
2. **Context suggestions.** On the log screen, show up to 3 memories ranked by: same meal type, similar time of day (±2 h), recency and frequency over the last 30 days. Pure ranking function in `packages/domain`, unit-tested.
3. **Matching new input.** After AI analysis or text parsing, if the candidate foods overlap a memory's items (at least 60% of items by food id), offer "Looks like your usual *Chicken plov*. Use your usual portion?".
4. **Corrections as context.** When analyzing a new meal, the Edge Function sends the user's 10 most relevant recent corrections and preparation preferences (for example "usually cooks with olive oil, about 1 tbsp") to the AI as context. The model output is still validated and still shown as an estimate.
5. **Clarifying questions.** Ask at most one question, only when the answer changes energy by more than 15% (for example oil amount). If the user has answered the same question for the same memory before, reuse the answer and skip the question.

## What v0 does not do
- No automatic logging without user confirmation.
- No inferring health conditions, no judgments about habits, no "good/bad" labels.
- No cross-user learning. Each user's memory is private.

## Success metrics (validation)
| Metric | v0 target for closed beta |
| --- | --- |
| Share of logs created from a memory or suggestion (week 3+) | ≥ 30% |
| Median time to confirmed log, repeat meals | ≤ 10 s |
| Share of AI estimates edited, week 1 vs week 4 | Falls by ≥ 20% relative |
| Users with ≥ 3 saved memories by day 14 | ≥ 40% |
If these are not met in beta, revisit the differentiation hypothesis before investing in learned personalization.

## Delivery
- Phase 2: memories, "same as last time", scaling, context suggestions (with text logging).
- Phase 3: matching after photo analysis, corrections as context, clarification reuse.
- Later (only if metrics justify it): learned portion priors, embeddings, personal models.
