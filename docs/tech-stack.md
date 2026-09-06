# MedRussia — Comprehensive Technology Stack

MedRussia is engineered with modern, industry-standard technologies selected for reliability, type safety, security, and developer productivity.

---

## 1. Backend Architecture

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **Python 3.11+** | Runtime | Modern Python runtime with enhanced exception groups and async performance. |
| **FastAPI 0.115+** | Framework | High-throughput asynchronous web framework implementing native ASGI and OpenAPI 3.1. |
| **Uvicorn 0.30+** | ASGI Server | Lightning-fast web server leveraging `uvloop` and `httptools`. |
| **Pydantic v2.8+** | Data Validation | High-speed Rust-backed schema validation and serialization. |
| **SQLAlchemy 2.0+** | ORM | Fully asynchronous declarative Object-Relational Mapping. |
| **asyncpg 0.29+** | DB Driver | Async PostgreSQL driver for direct, high-performance database communication. |
| **Alembic 1.13+** | Migrations | Automated schema migrations with forward/backward migration version control. |
| **structlog 24.4+** | Logging | Machine-readable, structured JSON logging with request correlation IDs. |
| **HTTPX 0.27+** | HTTP Client | Asynchronous client for external service integration (e.g., Google Gemini AI). |

---

## 2. Web Frontend Architecture

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **React 19 / 18** | UI Library | Component-driven Single Page Application (SPA) architecture. |
| **TypeScript 5.8+** | Language | Strictly typed frontend code ensuring compile-time reliability. |
| **Vite 6.2+** | Bundler & Tooling | Next-generation dev server with instant Hot Module Replacement (HMR). |
| **Tailwind CSS v3** | Styling | Utility-first CSS framework with responsive layouts and custom design tokens. |
| **React Router v6** | Routing | Declarative client-side routing, route guards, and deep-link handling. |
| **Recharts 3.6+** | Visualizations | Interactive SVG financial charts and comparative cost graphs. |
| **Lucide React** | Icons | Consistent, lightweight SVG icon system. |
| **Zod 4.4+** | Schema Validation | Declarative runtime validation for forms and client inputs. |

---

## 3. Mobile Native Architecture (Android)

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **Kotlin 1.9+** | Language | Modern concise language with first-class null safety and coroutines. |
| **Java 21 Toolchain** | JDK | JVM compilation environment. |
| **Jetpack Compose** | UI Toolkit | Declarative reactive UI framework utilizing Material Design 3. |
| **Kotlin Coroutines** | Concurrency | Asynchronous, non-blocking execution across Main, IO, and Default dispatchers. |
| **StateFlow & SharedFlow** | Reactive Streams | Hot observable streams managing UI state and one-shot events. |
| **Ktor Client** | Networking | Multiplatform HTTP client utilizing the OkHttp engine with Bearer Auth interceptors. |
| **Kotlinx Serialization** | Serialization | Pure-Kotlin reflectionless JSON parsing. |
| **Coil Compose** | Image Loading | High-performance asynchronous image rendering with disk/memory caching. |
| **R8 / ProGuard** | Optimization | Bytecode optimization, resource shrinking, and code minification for release builds. |

---

## 4. Database & Storage

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **PostgreSQL 15+** | Relational DB | ACID-compliant primary storage with foreign key cascades and B-tree indexing. |
| **Private S3-Compatible Storage** | Cloud Storage | Private cloud bucket architecture; objects accessible strictly via short-lived presigned URLs. |

---

## 5. Authentication & Security

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **JWT (PyJWT 2.9+)** | Tokens | Cryptographically signed access and refresh tokens (HMAC-SHA256). |
| **Argon2-cffi & Passlib** | Password Hashing | Memory-hard cryptographic hashing with dynamic salt generation. |
| **Role-Based Access Control (RBAC)**| Authorization | Permission tiers strictly isolating `student`, `counselor`, and `admin` capabilities. |
| **RFC 7807 Problem Details** | Error Standard | Standardized error serialization for consistent cross-client troubleshooting. |

---

## 6. Infrastructure & Deployment

| Technology | Category | Description / Purpose |
| :--- | :--- | :--- |
| **Docker** | Containerization | Multi-stage Docker packaging for reproducible backend deployment. |
| **Render.com** | Backend Hosting | Fully managed Linux container cloud web service with auto-scaling. |
| **Vercel** | Frontend Hosting | Global Edge CDN hosting the React/Vite single-page application. |
| **Google Gemini API** | AI Infrastructure | High-reasoning LLM engine powering the 24/7 AI MD advising proxy. |
