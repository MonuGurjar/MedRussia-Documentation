# MedRussia Android — Technical Architecture Deep Dive

## 1. Architectural Philosophy

MedRussia Android adheres to Google's official Modern Android Architecture guidelines, prioritizing:
* **Separation of Concerns**: Decoupling the visual presentation layer from state holders and network abstractions.
* **Driven UI from Data Models**: UI is a pure declarative transformation of state emitted by ViewModels:
  $$\text{UI} = f(\text{UiState})$$
* **Single Source of Truth**: Remote data fetched via the Central Platform API represents the authoritative state of admission applications, university fees, and KYC records.
* **Fault Tolerance & Resilient Networking**: Network interruptions trigger deterministic typed exceptions that are rendered cleanly without application crashes.

---

## 2. Layered Structure

```
+-------------------------------------------------------------------------+
|                              PRESENTATION LAYER                         |
|                                                                         |
|  +-------------------------------------------------------------------+  |
|  |                   Composable UI Hierarchy                         |  |
|  |   - AnimatedContent transitions between primary screen states     |  |
|  |   - Material Design 3 surface tokens and dynamic color schemes    |  |
|  |   - System BackHandler and WindowInsets management                |  |
|  +-----------------------------------+-------------------------------+  |
|                                      | User Intent Dispatches           |
|                                      v                                  |
|  +-------------------------------------------------------------------+  |
|  |                       AndroidX ViewModels                         |  |
|  |   - Holds immutable StateFlow<UiState>                            |  |
|  |   - Manages asynchronous coroutines within viewModelScope         |  |
|  |   - Encapsulates UI business logic and state transitions          |  |
|  +-----------------------------------+-------------------------------+  |
+--------------------------------------|----------------------------------+
                                       |
                                       v
+-------------------------------------------------------------------------+
|                                DATA LAYER                               |
|                                                                         |
|  +-------------------------------------------------------------------+  |
|  |                       Repository Layer                            |  |
|  |   - PlatformApplicationRepository: Manages dossiers & milestones  |  |
|  |   - PlatformAuthRepository: Handles login, registration & tokens  |  |
|  |   - UniversityRepository: Fetches catalog & fee structures        |  |
|  +-----------------------------------+-------------------------------+  |
|                                      |                                  |
|           +--------------------------+--------------------------+       |
|           v                                                     v       |
|  +-------------------------------+     +-----------------------------+  |
|  |     Remote Data Source        |     |     Local Data Source       |  |
|  |   - Ktor HTTP Engine          |     |   - SharedPreferences       |  |
|  |   - Kotlinx Serialization     |     |   - TokenManager (Mutex)    |  |
|  |   - Bearer Auth Interceptor   |     |   - User Preference Caches  |  |
|  +-------------------------------+     +-----------------------------+  |
+-------------------------------------------------------------------------+
```

---

## 3. Reactive UI State Modeling

UI state is modeled strictly with sealed class hierarchies to ensure exhaustive handling in Compose:

```kotlin
sealed class AuthUiState {
    object Idle : AuthUiState()
    object Loading : AuthUiState()
    data class Success(val user: UserProfileDto) : AuthUiState()
    data class EmailVerificationSent(val email: String) : AuthUiState()
    data class Error(val message: String) : AuthUiState()
}
```

This pattern prevents undefined UI states, guarantees compile-time safety, and eliminates invalid visual artifacts.

---

## 4. Resilient Networking & Token Lifecycle

Network communication is orchestrated by a centralized `PlatformApiClient`:
* **Automatic Header Injection**: Every authorized request receives `Authorization: Bearer <access_token>`.
* **Automatic 401 Interception**: When an endpoint returns HTTP 401 Unauthorized, the client initiates a background token refresh request, updates `TokenManager`, and seamlessly replays the original request.
* **Thread-Safe Token Manager**: Access and refresh tokens are protected by a Kotlin Coroutine `Mutex` to prevent concurrent write collisions during asynchronous requests.
