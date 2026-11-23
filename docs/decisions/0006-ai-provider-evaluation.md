# ADR 0006: AI provider selection by evaluation

- **Status:** Accepted (2026-11-23). Provider to be chosen by the evaluation below.
- **Context:** Meal analysis needs a vision-capable model with schema-constrained output. Cost per analysis is the largest variable cost. The founder has used Gemini on another project; that is not by itself a reason to choose it here.

## Decision

1. **Evaluate before choosing.** Compare 2–3 vision models, for example one each from Google (Gemini), Anthropic (Claude) and OpenAI, at their low-cost tiers first.
2. **Benchmark:** at least 100 meal photos across at least 8 cuisine groups, plus 50 text descriptions, with reference foods and approximate weights (ARCHITECTURE.md "Accuracy and evaluation"). Use founder-owned and consented photos only.
3. **Measure per cuisine:** food identification accuracy, portion error, calorie absolute error after deterministic resolution, schema-valid rate, p95 latency, cost per analysis.
4. **Selection bar for beta:** schema-valid ≥ 98%; p95 latency ≤ 10 s; cost ≤ US$0.01 per photo analysis; no cuisine group dramatically worse than the rest without a stated limitation in the app.
5. **Data terms:** production traffic must use API terms under which inputs are **not used for model training**. Free API tiers may be used only for development and benchmarking with non-user data, and only if their terms allow it.
6. **Architecture:** all calls go through the `AIProvider` interface in Edge Functions; output is validated with Zod; one provider plus a configured fallback. Text parsing may use a cheaper model than photo analysis.
7. **Cost controls:** free users get a daily photo-analysis limit (start at 5 per day; text logging is unlimited); global monthly AI budget alert; per-user rate limits.

## Consequences
- About one week of evaluation work in Phase 0/3; results become a short report in `docs/evaluations/`.
- Provider can change later without touching app code.

## Revisit when
Cost, accuracy or terms of the chosen provider change, or a new model clearly beats it on the benchmark.
