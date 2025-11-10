
## Tasks / Backlog

### Epics
- **E1 — Insurance ID input & validation**  
  Implement onboarding input screen, client‑side masking/validation, and secure handling of the identifier.

- **E2 — Multi‑step survey (5 screens)**  
  Wizard: Basic → Preferences → Goals → Activity → Start Date (date picker). Persist progress and support resume.

- **E3 — Bottom navigation (5 tabs)**  
  BottomNav with independent navigator stacks: Dashboard, Food Diary, Meals, Calendar, Profile.

- **E4 — Meals preset**  
  Seed / preload 5 preset meal cards and data model.

- **E5 — Telemetry & feature flag**  
  Telemetry events for onboarding flow and feature flag gating (`feature.onboarding.survey`) for staged rollouts [1].

- **E6 — Testing & A11y**  
  Unit, integration, E2E tests and accessibility (WCAG/A11y) validation.

---

### Tasks (examples)
- FE: OnboardingInput screen (+ mask / validation)  
- FE: Survey.Basic (+ persist step)  
- FE: Survey.Preferences  
- FE: Survey.Goals  
- FE: Survey.Activity  
- FE: Survey.Date (date picker)  
- FE: BottomNav + 5 tabs and routing (nested navigators)  
- FE: Meals preset (5 cards + seed script)  
- Telemetry events + feature flag checks (emit `onboarding_started`, `survey_step_{n}_completed`, `onboarding_completed`)  
- Unit tests (validation / serialization)  
- Integration tests (routing / resume state)  
- E2E test (see `testing.md`)  
- Docs impact: update `README.md`, `design.md`, `references.md`, `requirements.md`, `testing.md` — mandatory per docs‑as‑code enforcement [1]

---

### Estimates & Risk
- **Risk R2** — insufficient test coverage and A11y gaps.  
- **Mitigation**: add an E2E test covering the critical onboarding path (ID → complete survey → main screen) and include an A11y checklist in the DoD for each PR (keyboard navigation, focus order, contrast, ARIA labels) [1].

---

### Example PR checklist (to enforce DoR/DoD)
- [ ] Code builds and unit tests pass  
- [ ] Integration / E2E tests added or updated (critical path covered)  
- [ ] A11y checklist completed (keyboard, labels, contrast)  
- [ ] Feature flag used (`feature.onboarding.survey`) and default OFF in prod [1]  
- [ ] Diátaxis docs updated: `design.md`, `references.md`, `requirements.md`, `testing.md` [1]

