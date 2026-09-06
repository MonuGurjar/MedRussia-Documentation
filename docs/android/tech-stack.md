# MedRussia Android — Technology Stack & Dependencies

## 1. Core Platform & Build Configuration

| Component | Specification |
| :--- | :--- |
| **Language** | Kotlin 1.9+ |
| **JDK Version** | Java 21 Toolchain (`JavaVersion.VERSION_21`) |
| **Compile SDK** | 35 (Android 15) |
| **Target SDK** | 35 (Android 15) |
| **Min SDK** | 24 (Android 7.0 Nougat) |
| **Build System** | Gradle with Kotlin DSL (`build.gradle.kts`) |
| **Code Minification** | R8 / ProGuard (`proguard-android-optimize.txt`) |

---

## 2. Jetpack & UI Libraries

* **Jetpack Compose (Compose BOM)**: Modern declarative UI toolkit.
* **Material Design 3 (`androidx.compose.material3`)**: Modern Google design components and dynamic theming.
* **Material Icons Extended**: Full icon library for intuitive navigation and status indicators.
* **AndroidX Activity Compose**: Integration between Activities and Compose UI trees.
* **AndroidX Lifecycle Runtime KTX**: Lifecycle-aware coroutine scopes and view models.
* **Coil Compose (`io.coil-kt:coil-compose`)**: Asynchronous image loading with memory and disk cache management.

---

## 3. Networking, Concurrency & Serialization

* **Ktor Client (OkHttp Engine)**: Lightweight, asynchronous networking client with configurable interceptors.
* **Kotlinx Coroutines Android**: Structured asynchronous concurrency.
* **Kotlinx Serialization JSON**: High-speed, compiler-generated serialization avoiding runtime reflection.
* **Android SharedPreferences**: Lightweight key-value persistence for session tokens and user preferences.

---

## 4. Testing Frameworks

* **JUnit 4 (`junit:junit:4.13.2`)**: Unit testing engine.
* **Kotlinx Coroutines Test**: Testing utilities for coroutines and virtual time advancement.
* **AndroidX Test Runner**: Instrumentation test harness.
