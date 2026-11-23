# ADR 0005: Food data sources

- **Status:** Accepted (2026-11-23)
- **Context:** NutriMetra targets a global market (D4) on a near-zero budget. Nutrient totals must come from structured records, not LLM arithmetic. No single free source covers global home-cooked food.

## Decision

1. **USDA FoodData Central** is the base for generic foods and ingredients. It is free and public domain. Import a curated subset (Foundation and SR Legacy foods) into our `foods` and `food_nutrients` tables with source ids and version.
2. **Open Food Facts** is used for packaged foods by **live query only**, cached briefly on the device. Its records are not merged into our database until the ODbL share-alike and attribution obligations are reviewed. Attribution is shown wherever OFF data appears.
3. **Curated global dish library.** We author our own `recipes` for common dishes across cuisines (target 150 dishes for beta, covering at least 8 cuisine groups), built from USDA ingredients with documented yield factors. This is our own data and the main route to global coverage.
4. **Regional sources** (national food composition tables) are added later, one at a time, when validation shows demand and licensing is reviewed.
5. All sources sit behind a `FoodDataProvider` interface. Each nutrient value carries its source and version (ADR 0004).
6. **Coverage honesty:** the app states that coverage varies by cuisine, and benchmark results are reported per cuisine (D4).

## Consequences
- Zero licence cost; no share-alike risk to our database.
- Building the dish library is real work: about 1–2 weeks spread across Phases 2–3, and a nutrition reviewer should check it before public launch.
- Packaged-food lookups need network access.

## Revisit when
Validation shows a market with poor coverage, or OFF licensing is reviewed and merging becomes worthwhile.
