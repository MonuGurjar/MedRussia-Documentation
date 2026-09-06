<div align="center">

  <img src="assets/images/logo.png" alt="MedRussia Android Logo" width="160" />

  # MedRussia Android

  ### Native Mobile Client for Medical Admissions & University Tracking

  [![Kotlin](https://img.shields.io/badge/Kotlin-1.9+-7F52FF.svg?style=flat-square&logo=kotlin&logoColor=white)](https://kotlinlang.org)
  [![Jetpack Compose](https://img.shields.io/badge/UI-Jetpack%20Compose%20BOM-4285F4.svg?style=flat-square&logo=android&logoColor=white)](https://developer.android.com/jetpack/compose)
  [![Material 3](https://img.shields.io/badge/Design-Material%203-34A853.svg?style=flat-square)](https://m3.material.io)
  [![Target SDK](https://img.shields.io/badge/Target%20SDK-35%20(Android%2015)-orange.svg?style=flat-square)](https://developer.android.com)
  [![Min SDK](https://img.shields.io/badge/Min%20SDK-24%20(Android%207.0)-lightgrey.svg?style=flat-square)](https://developer.android.com)
  [![License](https://img.shields.io/badge/License-MIT-amber.svg?style=flat-square)](LICENSE)

  <p align="center">
    <strong>A modern, declarative Android application built with Jetpack Compose, Kotlin Coroutines, and Clean MVVM Architecture to guide Indian medical students through verified MBBS admissions in Russia.</strong>
  </p>

</div>

---

## 📌 Overview

**MedRussia Android** is the official native mobile application for the MedRussia ecosystem. Crafted with modern Android development best practices, it provides medical candidates and their parents with a fluid, high-performance interface for exploring 40+ accredited Russian universities, running 6-year multi-currency budget models, verifying NMC FMGL 2021 compliance, and monitoring real-time admission milestones.

> **Notice**: This public repository serves as an architectural and technical portfolio showcase. Production source code and proprietary backend integration keys are managed within our private enterprise repository (`medrussia-android 🔒`).

---

## 📱 Key Features & Screens

* **Cinematic Onboarding Flow**: Immersive multi-slide introduction detailing foreign medical studies, statutory NMC compliance, and Russian climate preparation.
* **Unified Student Dashboard**: Central home screen featuring an active admission milestone card, 4-tile quick actions grid, and featured university carousel.
* **40+ University Catalog**: Deep searchable catalog with instant filters for NMC recognition, tuition limits, and Indian dining facilities.
* **Interactive 6-Year Budget Calculator**: Real-time multi-currency calculator computing tuition, hostel, mess, health insurance, and annual flight expenses.
* **NMC FMGL 2021 Eligibility Evaluator**: Instant algorithmic compliance verification against statutory Indian medical regulations.
* **5-Stage Live Admission Dossier Tracker**: Visual milestone tracker reporting real-time progress from application review to MVD Ministry visa issuance and campus reception.
* **Private Student Document Vault**: Upload and preview sensitive marksheets and passport copies through secure, short-lived presigned URLs.
---

## 📥 Pre-Built APK Downloads (v1.1.5)

| Build Variant | Direct Download Link | Size | Details |
| :--- | :--- | :--- | :--- |
| **Release Build** | [⬇️ **Download `app-release-v1.1.5.apk`**](../../apks/app-release-v1.1.5.apk) | `14 MB` | Production-optimized, R8 minified & release signed. |
| **Debug Build** | [⬇️ **Download `app-debug-v1.1.5.apk`**](../../apks/app-debug-v1.1.5.apk) | `37 MB` | Development build with diagnostic logging enabled. |

---

## 🏛️ Architecture & Design Patterns

The application follows the recommended **Android Modern App Architecture**:

```
+-------------------------------------------------------------+
|                         UI LAYER                            |
|                                                             |
|   +-----------------------------------------------------+   |
|   |         Jetpack Compose UI Screens (Views)          |   |
|   |    - HomeScreen, AdmissionTrackerScreen, Vault      |   |
|   |    - Material 3 Design System & Edge-to-Edge        |   |
|   +--------------------------^--------------------------+   |
|                              | Emits StateFlow (UDF)        |
|   +--------------------------+--------------------------+   |
|   |             AndroidX ViewModels (State Holders)     |   |
|   |    - AuthViewModel, UniversityViewModel             |   |
|   |    - Coroutine Scopes (viewModelScope)              |   |
|   +--------------------------^--------------------------+   |
+------------------------------|------------------------------+
                               |
+------------------------------|------------------------------+
|                        DATA LAYER                           |
|                                                             |
|   +--------------------------+--------------------------+   |
|   |             Repository Implementations              |   |
|   |    - PlatformApplicationRepository                  |   |
|   |    - AuthRepository, UniversityRepository           |   |
|   +--------------------------^--------------------------+   |
|                              |                              |
|   +--------------------------+---+----------------------+   |
|   | Local State (SharedPreferences) | Remote API Client    |   |
|   | - TokenManager (Mutex/Volatile) | - Ktor HTTP Engine   |   |
|   | - UserPrefs                     | - Bearer Auth / JSON |   |
|   +---------------------------------+-------------------+   |
+-------------------------------------------------------------+
```

### Key Engineering Principles
1. **Unidirectional Data Flow (UDF)**: ViewModels emit immutable UI State (`StateFlow<UiState>`); Composables observe state and dispatch user events back to the ViewModel.
2. **Repository Pattern**: All network requests and local caches are mediated through clean repository interfaces.
3. **Thread Safety & Token Synchronization**: `TokenManager` utilizes Kotlin `Mutex` locks and `@Volatile` memory barriers to ensure safe token retrieval during concurrent network dispatches.
4. **RFC 7807 Error Deserialization**: All backend errors are parsed into typed Kotlin data models (`ProblemDetails`) for precise user-facing error feedback.

---

## 🛠️ Technology Stack

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Language** | Kotlin 1.9+ | Modern, expressive language with strict null safety and coroutines. |
| **Toolchain** | Java 21 | High-performance modern JDK compilation target. |
| **UI Toolkit** | Jetpack Compose (BOM) | Declarative UI framework eliminating XML layouts. |
| **Design System** | Material Design 3 | Latest Android design components (`androidx.compose.material3`). |
| **Concurrency** | Kotlin Coroutines | Structured concurrency across `Dispatchers.Main`, `IO`, and `Default`. |
| **Reactive State** | StateFlow & SharedFlow | Hot observable streams for reactive UI updates and one-shot events. |
| **Networking** | Ktor Client | High-throughput asynchronous HTTP client with OkHttp engine. |
| **Serialization** | Kotlinx Serialization | Fast, compiler-generated JSON parsing. |
| **Image Loading** | Coil Compose | Asynchronous image decoding with memory and disk caching. |
| **Build Optimization** | R8 / ProGuard | Bytecode minification, resource shrinking, and code obfuscation. |

---

## 🔒 Security & Privacy

* **Zero Hardcoded Secrets**: All endpoint URLs and keys are injected at build time via `local.properties` or CI/CD environment variables.
* **Hardware-Backed Keystores**: Production APKs are signed with release keystores kept strictly outside public version control.
* **Stateless Token Management**: Access tokens and refresh tokens are managed securely and rotated automatically upon 401 response challenges.
* **Encrypted Sandbox Previewing**: Sensitive documents retrieved from the private Document Vault are opened via ephemeral signed URLs with short 15-minute lifespans.

---

## 📂 Repository Structure

```
MedRussia-Android/
│
├── README.md               # Public project showcase & architecture overview
├── LICENSE                 # MIT License
│
├── docs/
│   ├── android-architecture.md  # Detailed Compose & state management breakdown
│   ├── android-features.md      # Screen-by-screen UX documentation
│   └── android-tech-stack.md    # Detailed dependency matrix
│
├── screenshots/
│   └── README.md                # Android screen captures and device previews
│
└── assets/
    └── images/                  # Icons and branding assets
```

---

## 📄 License

This repository is distributed under the **MIT License**. See [LICENSE](LICENSE) for more details.

---

<div align="center">
  <sub>Built with modern Android engineering for tomorrow's doctors. © 2026 MedRussia Technologies.</sub>
</div>
