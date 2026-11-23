# ADR 0001: Cross-platform mobile stack

- **Status:** Accepted (2026-11-23)
- **Decision:** Expo + React Native + TypeScript.
- **Context:** Solo founder needs simultaneous iOS/Android delivery, rapid product iteration and strong access to camera and notifications, and a path to health APIs after MVP.
- **Alternatives:** Flutter; Kotlin Multiplatform; separate Swift and Kotlin apps.
- **Rationale:** Existing TypeScript/React skills reduce delivery risk; Expo supports native builds and config plugins while keeping most product code shared.
- **Delivery order:** Android first, then iOS (ADR 0003).
- **Consequences:** Validate camera, image upload, background tasks and HealthKit/Health Connect needs on physical devices early. Use development builds rather than relying solely on Expo Go.
- **Revisit when:** measured platform limitations materially affect reliability, UX or performance.
