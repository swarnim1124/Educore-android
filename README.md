# EduCore Android

EduCore Android is a modular ERP mobile app built with Kotlin, Jetpack Compose, and a feature-based multi-module architecture.

## Features

- Student-focused ERP flows including dashboard, profile, exams, attendance, curriculum, timetable, and fees.
- Modern Compose UI with Navigation Compose.
- Dependency injection with Hilt.
- Dynamic feature module setup for major ERP domains.
- Firebase integrations (Analytics, Crashlytics, Performance Monitoring).

## Screenshots

### Student Dashboard
![Student Dashboard](images/dashboard.png)

### Exams
![Exams](images/exams.png)

### Hall Ticket
![Hall Ticket](images/hall-ticket.png)

### Manage Pinned Modules
![Manage Pinned Modules](images/pinned-modules.png)

## Tech Stack

- **Language:** Kotlin
- **UI:** Jetpack Compose + Material 3
- **Architecture:** Multi-module Android app with dynamic feature modules
- **DI:** Hilt
- **Networking:** Retrofit + OkHttp
- **Local data:** DataStore, Room
- **Build tools:** Gradle (Kotlin DSL), Android Gradle Plugin

## Project Architecture (high level)

- `:app` — application shell and main navigation wiring
- `:core` + `:core:navigation`, `:core:permissions` — shared foundation modules
- `:feature:*` — domain features such as login, dashboard, profile, exam, attendance, curriculum, timetable, fee
- `:sdk:*` — reusable SDK-style modules

## Setup

### Prerequisites

- Android Studio (recent stable)
- JDK 17+
- Android SDK with API 35 target support

### Build and run

```bash
./gradlew :app:assembleDebug
```

Then run the app module from Android Studio on an emulator or connected device.

## Signing Configuration (Release)

For release signing, create `keystore.properties` at the repository root (see `keystore.properties.example`) or set equivalent `RELEASE_*` environment variables.
