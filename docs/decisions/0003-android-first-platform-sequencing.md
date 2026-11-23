# ADR 0003: Android-first delivery from one shared codebase

- **Status:** Accepted (2026-11-23)
- **Decision:** Build one Expo + React Native codebase for Android and iOS. Develop on Windows, test and release on Android first (Google Play), verify iOS through EAS cloud builds until a Mac is available, then run a dedicated Mac-based iOS enablement milestone.
- **Context:** The founder develops on Windows (Cursor, PowerShell, npm) and already has a Google Play Developer account. The iOS Simulator, Xcode and local iOS builds require macOS. A MacBook and an iPhone will be bought within about two months (by late January 2027), before the Android release. An Apple developer account will be created when the iOS milestone starts.
- **Alternatives:**
  - Wait for a Mac before starting. Rejected: idles weeks of low-regret Android and backend work.
  - Rent a cloud Mac now. Kept as fallback if the Mac purchase slips.
  - Simultaneous iOS and Android release. Rejected for now: cannot be tested properly on Windows.

## Temporary limitations (until a Mac is available)
- No Xcode, iOS Simulator, Instruments or native iOS debugging.
- iOS builds only on EAS. An EAS simulator build proves iOS compiles but cannot be run.
- Physical iPhone builds require a paid Apple Developer membership and an iPhone.
- iOS-only behaviour is unverified: permission prompts, HEIC photos, safe areas and keyboard, back-swipe navigation, Sign in with Apple, APNs push, VoiceOver and dynamic type.

## Platform-neutrality rules (Android-first must not complicate iOS)
1. Use Continuous Native Generation. Do not commit or hand-edit `android/` or `ios/`; all native configuration goes through `app.config.ts` and config plugins.
2. Add a dependency only if it supports both Android and iOS (check Expo SDK support or the library's platform list).
3. Keep business logic in `packages/domain` and `packages/contracts`, free of React Native and platform APIs.
4. Platform-specific code lives only in small adapters. Any `*.android.tsx` file needs an `*.ios.tsx` counterpart or a shared default, and `Platform.OS` checks stay out of domain and service code.
5. Configure iOS identifiers and permission usage strings from day one.
6. Design for both platforms: safe areas, keyboard avoidance, Android hardware back and iOS back-swipe, dynamic type on both.
7. Normalize uploaded images to JPEG so iOS HEIC photos work without pipeline changes.
8. Keep auth provider-agnostic so Sign in with Apple can be added without restructuring. The Android release may ship without it.
9. Write Maestro E2E flows to run on both platforms.
10. Every milestone exit includes "EAS iOS simulator build compiles".

## Transition to Mac-based iOS development
Trigger: Mac available. Steps: install Xcode and run the iOS Simulator build; create a physical iPhone development build; run unit, integration and Maestro suites on iOS; fix gaps in shared code; add Sign in with Apple, APNs and the privacy manifest; set up App Store Connect, TestFlight, privacy labels and RevenueCat iOS products. Exit: iOS parity with the current Android milestone and a running TestFlight beta.

## Consequences
- Android users get the product first; iOS users wait for the iOS enablement milestone.
- iOS defects found late are cheaper because of the neutrality rules and compile checks, but not free; budget 2–3 weeks.
- Revisit if the Mac purchase slips beyond the Android release; then use a cloud Mac.
