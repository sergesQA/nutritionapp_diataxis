# Design / Explanations

## Architecture (Flutter)

- **Navigation**
  - Use GoRouter / Router API.
  - Route stacks:
    - `/onboarding` → `/survey/*` → `/main` (tabs).
  - Each bottom tab should have its own nested navigator stack to preserve independent navigation state.

- **State management**
  - App-wide: Riverpod (recommended) or Provider.
  - Local / per-screen: StateNotifier / ChangeNotifier for isolated state and controlled rebuilds.

- **Survey forms**
  - Multi-step wizard with 5 screens:
    1. Basic (height, weight, age)
    2. Preferences (dietary preferences)
    3. Goals (health goals)
    4. Activity (activity level)
    5. Start Date (date picker)
  - Persist progress locally (securely) so the user can resume.

- **BottomNavigationBar**
  - Five tabs (right → left): Dashboard, Food Diary, Meals, Calendar, Profile.
  - Each tab: independent navigator + lazy loading of heavy content.

- **Data**
  - `insurance_id`
    - Keep only in ephemeral memory during onboarding.
    - If persistence is necessary, store masked/hashed form (SHA‑256 + salt) using device secure storage.
    - Never commit actual IDs to repo or logs.
  - Survey results
    - POST to backend API (TLS).
    - Cache locally in `SharedPreferences` / `flutter_secure_storage` while in progress or when offline.
    - Sync on network availability (with conflict resolution policy).

- **Accessibility (A11y)**
  - Ensure sufficient color contrast.
  - Manage focus order and screen reader labels.
  - Touch targets ≥ 44px.
  - Keyboard navigation where applicable.

- **Telemetry**
  - Events to emit:
    - `onboarding_started`
    - `survey_step_{n}_completed` (n = 1..5)
    - `onboarding_completed`
  - Include feature‑flag context and anonymized user metrics (no PHI in telemetry).

---

## Feature flag
- Key: `feature.onboarding.survey`
  - Check flag on entry to `/onboarding` and when rendering “Start” / “Complete” buttons.
  - Log flag state (enabled/disabled) for audit and rollout analysis.
- Rollout guidance: default OFF in production, ON in dev/stage; use staged rollout and rollback criteria per the delivery workflow [1]. [1]

---

## Security
- Do **not** store real Insurance IDs in the repository, test fixtures, or logs. Use placeholders (e.g., `test_account_001`) or hashed values.
- On device: use `flutter_secure_storage` for any persisted sensitive values.
- In transit: TLS for all API calls.
- On server: enforce RBAC and server-side encryption; redact identifiers in logs.

Example: simple hashing (Dart pseudo-code)
```dart
import 'dart:convert';
import 'package:crypto/crypto.dart';

String hashInsuranceId(String id, String salt) {
  final bytes = utf8.encode('$salt:$id');
  return sha256.convert(bytes).toString();
}
```
Store only `hashInsuranceId(id, salt)` in device storage if needed.

---

## Screen states & error handling
- **Empty / invalid ID**
  - `Continue` button disabled until validation passes.
- **Network unavailable**
  - Save survey progress locally; show retry / sync UI.
- **Re-entering survey**
  - Resume from the last completed step using persisted progress.
- **Server errors**
  - Show friendly error messages and provide retry/back options; capture anonymized error telemetry.

---

## Screenshots
(See `README/testing` for embedded images; place screenshots under `assets/screenshots/`)

- Insurance ID:
   ![Insurance ID](/assets/screenshots/01_onboarding_id.png)   
- Survey — Basic:
   ![Survey — Basic](/assets/screenshots/02_survey_basic.png)  
- Survey — Preferences:
   ![Survey — Preferences](/assets/screenshots/03_survey_preferences.png)  
- Survey — Goals:
   ![Survey — Goals](/assets/screenshots/04_survey_goals.png)  
- Survey — Activity:
   ![Survey — Activity](/assets/screenshots/05_survey_activity.png)  
- Survey — Date:
   ![Survey — Date](/assets/screenshots/06_survey_date.png)  
- Dashboard:
   ![Dashboard](/assets/screenshots/07_dashboard.png)  
- Profile:
   ![Diary](/assets/screenshots/11_diary.png)  
- Calendar:
   ![Meals](/assets/screenshots/10_meals.png)  
- Meals:
   ![Calendar](/assets/screenshots/09_calendar.png)  
- Diary:
   ![Profile](/assets/screenshots/08_profile.png)


Embed example:
```markdown
![Onboarding - Enter Insurance ID](../../assets/screenshots/01_onboarding_id.png)
