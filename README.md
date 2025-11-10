
# Onboarding — Insurance ID → Dietitian Survey → Main Navigation

## Brief

Этот раздел описывает онбординг‑флоу: от ввода идентификатора до попадания на главный экран приложения с нижней навигацией.

https://github.com/sergesQA/nutritionapp

https://v0-nutrition-tracking-app-git-demo-seges-projects-2d2c87ab.vercel.app/

## Goal
- Провести авторизованного пользователя через ввод идентификатора (placeholder: `insurance_id`),
- Запустить и пройти 5‑шаговый опрос диетолога (dietitian survey),
- После завершения — перенаправить пользователя на главный экран с нижней панелью навигации:
  - Profile
  - Calendar
  - Meals
  - Diary
  - Dashboard

## Diátaxis (structure)
- Tutorials  
  - `testing.md` — E2E сценарий, пошагово с скриншотами
- How‑to / Requirements  
  - `requirements.md` — требования и оперативные инструкции
- Reference  
  - `references.md` — API, модели данных, маршруты
- Explanations  
  - `design.md` — архитектура, решения и rationale

## User story
> “A user who is already authenticated enters a string (`insurance_id`), taps the button and lands in the dietitian survey (with a date selector). After completing the survey, the user is taken to the main screen. Bottom navigation (right → left): Dashboard, Food Diary, Meals (5 preset meals), Calendar, Profile.”

## Screenshots

- Insurance ID:
- ![Insurance ID](/assets/screenshots/01_onboarding_id.png)   
- Survey — Basic:
- ![Survey — Basic](/assets/screenshots/02_survey_basic.png)  
- Survey — Preferences:
- ![Survey — Preferences](/assets/screenshots/03_survey_preferences.png)  
- Survey — Goals:
- ![Survey — Goals](/assets/screenshots/04_survey_goals.png)  
- Survey — Activity:
- ![Survey — Activity](/assets/screenshots/05_survey_activity.png)  
- Survey — Date:
- ![Survey — Date](/assets/screenshots/06_survey_date.png)  
- Dashboard:
- ![Dashboard](/assets/screenshots/07_dashboard.png)  
- Profile:
-  ![Diary](/assets/screenshots/11_diary.png)  
- Calendar:
- ![Meals](/assets/screenshots/10_meals.png)  
- Meals:
- ![Calendar](/assets/screenshots/09_calendar.png)  
- Diary:
- ![Profile](/assets/screenshots/08_profile.png)

```

## Feature flag
- Key: `feature.onboarding.survey`  
  - Default: OFF in production.  
  - Environments: ON in dev/stage.  
  - Rollout: staged rollout (5% → 25% → 50% → 100%) — follow staged rollout gating and rollback criteria in the delivery workflow [1]. [1]

## Acceptance criteria
- AC1 — The onboarding screen accepts a non-empty identifier; Continue button enabled only when input is valid.
- AC2 — The survey is a 5-step wizard (Basic → Preferences → Goals → Activity → Start Date) with Next/Back controls.
- AC3 — Start Date uses a date picker and accepts any valid date in locale format.
- AC4 — On Complete, the user is routed to the main application UI where the bottom nav shows: Dashboard, Food Diary, Meals, Calendar, Profile.
- AC5 — Meals screen contains 5 seeded/preset meals.
- AC6 — Telemetry events emitted: `onboarding_started`, `survey_step_completed`, `onboarding_completed`.
- AC7 — No real PHI (insurance IDs / survey answers) is committed to repo/files — only placeholders or hashed identifiers.

## Flow (high level)
1. Authenticated user opens onboarding screen (or arrives from SSO).
2. User enters `insurance_id` (masked in UI logs) → taps Continue.
3. App opens survey wizard (5 steps). Each step persisted locally (SecureStorage) until Complete.
4. User picks Start Date using date picker (calendar).
5. On Complete: submit survey to API (over TLS) and navigate to `/main` with bottom navigation.

## Implementation notes (Flutter)
- Navigation: use GoRouter or Navigator 2.0 with nested navigation for bottom tabs.
- State: Riverpod / Provider for app state; persist survey progress to `flutter_secure_storage` while in progress.
- UI: multi-step wizard widgets; DatePicker uses platform picker where available.
- Tests:
  - Unit: validation for `insurance_id`, serialization of `SurveyResponse`.
  - Integration: navigation flow, persistence/resume behavior.
  - E2E: real device/emulator test that runs the full onboarding path.
- Docs: update Diátaxis files (`design.md`, `references.md`, `requirements.md`, `testing.md`) when implementation changes.

## Security & PHI
- Do not store real insurance IDs in code, test fixtures, or screenshots. Store only masked or hashed IDs (e.g., HMAC/SHA256 + salt) if persisted.
- Use TLS, server-side RBAC, and encrypted storage on device.
- Log only masked identifiers and avoid exposing survey answers in logs.

## References (MANDATORY in every artifact)
- Security / Privacy: link to canonical security and data classification standards (company standards repo).  
- Architecture / ADRs: list any relevant ADR files.  
- Testing policy / CI gates: link to the CI gating rules (coverage, SAST, A11y checks).  
- This References block is required by docs-as-code enforcement and must be present in each feature doc [1]. [1]

## File map (for reviewers)
- `design.md` — architecture, rationale, PHI controls  
- `references.md` — API endpoints, models, config keys  
- `requirements.md` — detailed functional & non-functional requirements, ACs  
- `tasks.md` — backlog & implementation tasks  
- `testing.md` — E2E tutorial, test commands, and expected results
