# EduCore Android

Native Android client for the **EduCore ERP** platform (see the companion FastAPI backend). It gives students and staff access to profile, timetable, attendance, exams, curriculum and fee payments.

**Stack:** Kotlin · Jetpack Compose · Hilt · Retrofit · Room · Coroutines/Flow · Firebase (Crashlytics/Perf) · Gradle Kotlin DSL · multi-module Clean Architecture

---

## Highlights

- **22-module Gradle project.** Feature modules are isolated from each other and depend only on shared SDKs and `:core`.
- **Clean Architecture per feature.** Each feature has `ui` (Compose screens, ViewModels, state/events), `domain` (use cases, repository interfaces) and `data` (repository implementations, APIs).
- **Reusable SDK layer.** Network, auth/session, theming, common UI, QR, camera, media, file and chat live in `sdk/`, independent of features.
- **Secure by default.** Token/session manager, certificate pinning support, reCAPTCHA, release signing via `keystore.properties` or CI env vars, R8 shrinking, and a build guard that blocks release builds without an explicit backend URL.
- **White-label ready.** `:branding`, `:config` and `:plugins` modules for per-tenant theming, environments and a feature-plugin registry.
- **Tested.** ~367 unit tests across ViewModels, use cases and repositories.

## Architecture

```text
app/                     # Application shell, navigation host, DI wiring
feature/
  login/  dashboard/  profile/  timetable/
  attendance/  exam/  curriculum/  fee/
core/                    # Shared base, navigation, permissions, observability
sdk/
  XscNetworkSDK  XscAuthSDK  XscThemeSDK  XscCommonUI
  XscQRCodeSDK   XscCameraSDK  XscMediaSDK  XscFileSDK  XscChatSDK
branding/  config/  plugins/
```

Data flow per feature: `Screen → ViewModel (state/events) → UseCase → Repository → API/DB`.

The backend uses a single dynamic endpoint (`POST /` with `mod / subMod / action / data`); `XscNetworkSDK` wraps this in a typed API client.

## Features

| Module | What it does |
|---|---|
| `login` | Sign-in, OTP verification, forgot/reset password, captcha |
| `dashboard` | Role-aware home and navigation |
| `profile` | Personal, family, academic and medical details |
| `timetable` | Class schedules, substitutions, export |
| `attendance` | Records, leave, shortage alerts |
| `exam` | Schedules, hall tickets, results |
| `curriculum` | Programmes, courses, enrollment |
| `fee` | Invoices and online payments |

## Getting started

Requirements: Android Studio (latest stable), JDK 17.

```bash
git clone <your-repo-url> && cd <repo>

# Point the app at your backend (debug builds default to https://dev.example.com/)
./gradlew assembleDebug -PbaseUrl=https://your-backend.example.com/

./gradlew test        # run unit tests
```

Release builds require an explicit `-PbaseUrl=...` and signing config. Copy `keystore.properties.example` to `keystore.properties`, or set the `RELEASE_*` env vars.

Firebase (Crashlytics/Performance) is optional. Add your own `app/google-services.json` and enable the plugins in `app/build.gradle.kts`.

## Documentation

- [Backend endpoint requirements](docs/BACKEND_ENDPOINT_REQUIREMENTS.md)
- [Production readiness log](docs/PRODUCTION_READINESS.md)

## My contributions

> Edit this to match what you actually built.

- _<modules / features you owned, e.g. login, profile, fee>_
- _<architecture, testing, CI, performance work>_

## Credits

Developed collaboratively as part of an education ERP platform. Add contributor names here.

## License

Add a LICENSE file before publishing.
