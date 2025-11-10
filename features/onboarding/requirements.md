
## Requirements / How‑to

### Functional requirements
- An authenticated user sees the insurance_id input screen (placeholder text).  
- The Continue button is enabled only for a valid non‑empty input.  
- Survey is a 5‑step wizard:
  1. Basic (height, weight, age)
  2. Preferences (dietary preferences)
  3. Goals (health/fitness goals)
  4. Activity (activity level)
  5. Start Date (with native DatePicker)
- On completion the app navigates to the main screen; bottom navigation contains 5 tabs.  
- Meals screen is preloaded/seeded with 5 meals.

### Non‑functional requirements
- Accessibility (A11y): meet WCAG AA where applicable — labels, focus order, contrast, touch targets.  
- Privacy: mask/hash identifiers (do not store raw IDs in repo); encrypt sensitive data in transit and at rest.  
- Reliability: support offline survey caching and resume from the last completed step.  
- Observability: emit telemetry events for survey stages (onboarding_started, survey_step_{n}_completed, onboarding_completed).

### Acceptance Criteria
- AC1 — Valid non‑empty input enables Continue; invalid/empty input keeps Continue disabled.  
- AC2 — DatePicker accepts any valid date; display uses locale format.  
- AC3 — After Complete, all 5 bottom tabs are visible and routing works for each.  
- AC4 — Meals page displays exactly 5 preset meals.  
- AC5 — `onboarding_completed` metric is recorded in telemetry.

### How‑to (run example locally)
- Install dependencies: `flutter pub get`  
- Run on device/emulator: `flutter run -d <device>`  
- Run integration/e2e tests: `flutter test integration_test` (configure devices/emulators as CI requires)

### Process (mandatory)
- Phase 0: Assessment‑first — read existing docs and references before starting work (Phase 0 assessment required) [1].  
- Feature flag: Implement under `FEATURE_ONBOARDING_SURVEY` — Default OFF in production; staged rollout 5% → 25% → 50% → 100% per rollout gates [1].  
- DoR / DoD: Use the Definition of Ready and Definition of Done checklists from the team workflow (`workflow_agile_ai.md`) when creating tickets and PRs — docs update is required as part of DoD [1].
