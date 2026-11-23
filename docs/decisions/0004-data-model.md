# ADR 0004: Core data model for estimates, provenance and dishes

- **Status:** Accepted (2026-11-23)
- **Context:** The product promises per-value provenance, visible uncertainty, deterministic totals and support for mixed home-cooked dishes. The initial model in ARCHITECTURE.md stored provenance per meal item and had no recipe or range fields. The backend runs on the Supabase free plan (ADR 0002), so the schema must also stay small.

## Decision

1. **Estimates are ranges.** Every estimated quantity stores `qty_low`, `qty_point`, `qty_high` and `unit`. Database or label values set all three equal. UI shows the point value and a range cue when low ≠ high.
2. **Provenance per value, stored compactly.** Each `meal_items` row has a `quantity_source` and each nutrient snapshot row has a `source`, both from one enum: `database`, `label`, `user`, `model_estimate`, `memory`. A `confidence` (0–1) is stored for `model_estimate` only.
3. **Nutrient snapshots, lean.** Core nutrients are typed columns on a `meal_item_nutrients` row (energy_kcal, protein_g, carbs_g, fat_g, fiber_g, sugar_g, sodium_mg, plus low/high for energy). Optional micronutrients go into one JSONB column only when present. Snapshots never change when catalog data changes.
4. **Dishes and recipes.** New tables `recipes` (owner: system or user) and `recipe_components` (food reference, raw quantity, unit, preparation). A `yield_factor` per recipe converts raw ingredient weight to cooked weight. Logging a dish expands to components so totals stay deterministic.
5. **Analysis runs are audit records, not data stores.** `meal_analysis_runs` keeps provider, model, prompt version, input hash, validated structured result, latency and cost. No raw images; photos are deleted after analysis.
6. **Meal memory** tables follow the Meal Context Engine v0 spec (`docs/MEAL_CONTEXT_ENGINE.md`): `meal_memories`, `meal_memory_items`, `user_corrections`.
7. **Conventions:** UUID primary keys, `created_at`/`updated_at`, explicit units, user-local date stored with time zone, indexes on `(user_id, local_date)`, RLS on every user-owned table, migrations only.

## Consequences
- Slightly more schema up front; avoids migrating confirmed history later.
- Totals are always sums of snapshots, computed in `packages/domain` and verified by unit tests.
- Storage estimate per logged item stays around a few hundred bytes, which keeps the free plan viable longer.

## Revisit when
Micronutrient features become core, or storage measurements differ materially from estimates.
