
## Reference

### Routes (Flutter)
```
/onboarding

/survey/basic
/survey/preferences
/survey/goals
/survey/activity
/survey/date

/main/dashboard
/main/diary
/main/meals
/main/calendar
/main/profile
```

### Data models (example)
```ts
OnboardingInput {
  insuranceIdMasked: string
}

SurveyResponse {
  heightCm: number
  weightKg: number
  age: number
  preferences: string[]   // e.g., ["Vegetarian", "Keto"]
  goals: string[]         // e.g., ["Weight Loss"]
  activityLevel: string   // e.g., "Moderate"
  startDate: string       // ISO8601 date
}

Meal {
  id: string
  title: string
  calories: number
  proteins: number
  carbs: number
  fats: number
}
```

### API (if backend is used)
- `POST /api/onboarding/start`  
  Body: `{ "insurance_id_masked": "<string>" }`

- `POST /api/survey/submit`  
  Body: `{ "survey_response": { ...SurveyResponse } }`

- `GET /api/meals/preset` → returns 5 meals (seeded)

- `GET /api/dashboard/summary` → user dashboard summary

- `GET /api/diary?date=YYYY-MM-DD` → food diary for the given date

### Config
- `FEATURE_ONBOARDING_SURVEY=true|false`  
- `API_BASE_URL`  
- `SECURE_STORAGE_KEY` (for device encryption / secure storage)

### Tools / Libraries
- Flutter SDK (stable)  
- State: Riverpod (recommended) or Provider  
- Localization: `intl`  
- Local storage: `shared_preferences` (non-sensitive), `flutter_secure_storage` (sensitive)  
- Tests: `flutter_test`, `integration_test`
