# NutriMetra — Eat with clarity

**Product:** AI-assisted food understanding and nutrition companion  
**Platforms:** Android first, then iOS (one shared codebase)  
**Working brand name:** NutriMetra (subject to trademark, domain and store clearance)  
**Status:** Pre-development / discovery

NutriMetra is designed to make nutrition tracking feel less like data entry and more like understanding your food. The product should combine fast meal capture with transparent estimates, contextual learning, and practical next-step guidance.

## Product thesis

Most calorie trackers optimize the act of logging. NutriMetra should optimize the user's ability to understand a meal and make a better next decision.

**North-star experience:** Capture a meal in seconds → see a transparent nutrition estimate → correct anything important → understand how it fits the day → receive one useful, non-judgmental suggestion.

The differentiating system is the **Meal Context Engine**:
- It remembers user-approved recurring meals, portions, preparation styles and corrections.
- It asks a short clarification only when it would materially change the estimate.
- It presents a range/confidence cue where visual estimation is uncertain.
- It learns from explicit user corrections without silently treating guesses as facts.
- It translates the day's nutrition into one practical next action, not a wall of generic advice.

This is a product hypothesis to validate, not a claim that AI can precisely infer calories from every photo.

## Start here

1. Read `CLAUDE.md` before making changes.
2. Read `docs/DECISIONS.md`, `docs/PRD.md` (canonical MVP scope), `docs/ARCHITECTURE.md` and `docs/MEAL_CONTEXT_ENGINE.md`.
3. Use `docs/ROADMAP.md` to select the first milestone.
4. Create an implementation plan before coding. Do not scaffold the entire product in one pass.
5. Keep all credentials in environment variables and never commit secrets.

## Repository conventions

- TypeScript strict mode.
- Feature-oriented modules; avoid giant screens and generic `utils.ts` dumping grounds.
- Domain logic must be testable without React Native.
- Validate all network payloads at runtime.
- Use migrations for database changes.
- Every AI estimate is editable and traceable to its inputs and source.
- No medical diagnosis, treatment claims, or shame-based language.
- Keep an ADR for material architecture decisions.

## Current status

This starter kit defines the product direction and engineering guardrails. It is not a claim that app-store, domain, trademark, nutrition-data licensing, or regulatory checks are complete.
