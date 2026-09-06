# MedRussia Platform — Backend Architecture & Service Design

## 1. Architectural Philosophy

MedRussia Platform is engineered to serve as the unified, authoritative backend for all client applications. It adheres to three design tenets:
1. **Asynchronous Non-Blocking Execution**: Utilizing Python's `asyncio` event loop alongside `uvloop`, `asyncpg`, and SQLAlchemy 2.0 Async ensures high concurrent throughput with minimal memory footprint.
2. **Dependency Injection (DI)**: Business services and database sessions are injected via FastAPI's native `Depends()` mechanism, simplifying unit testing and promoting clean separation of concerns.
3. **Domain-Driven Service Layer**: Routes act exclusively as protocol adapters (deserializing HTTP requests and serializing responses). Core business logic, statutory validations, and state transitions reside inside isolated domain services (`ApplicationService`, `UniversityService`, `AuthService`).

---

## 2. Request Lifecycle Pipeline

```
Incoming Client Request (HTTPS)
   ↓
Middleware: Request ID Injection (`X-Request-ID`)
   ↓
Middleware: CORS Validation
   ↓
FastAPI Router Matching (`/api/v1/...`)
   ↓
Dependency Injection: Database Session (`get_db`) & Auth Guard (`get_current_user`)
   ↓
Pydantic v2 Payload Validation (422 RFC 7807 on failure)
   ↓
Domain Service Execution (`service.submit_application(...)`)
   ↓
SQLAlchemy 2.0 Async Query via `asyncpg`
   ↓
PostgreSQL ACID Transaction Commit
   ↓
Response Envelope Serialization (`ResponseEnvelope[T]`)
   ↓
Structured JSON Logging via `structlog`
   ↓
Outgoing HTTP 200/201 Response to Client
```

---

## 3. Core Domain Services

### 3.1 AuthService & Identity Manager
* Generates asymmetric/symmetric signed JSON Web Tokens (JWT).
* Issues short-lived access tokens (15 minutes) and rotates refresh tokens (30 days).
* Verifies passwords against salted Argon2-cffi hashes.

### 3.2 ApplicationService (5-Stage Dossier State Machine)
* Validates admission candidate criteria.
* Generates standardized dossier identification numbers (`RU-2026-XXXX`).
* Manages progression across the 5 official milestones:
  - `APPLIED`
  - `ADMISSION_LETTER_ISSUED`
  - `INVITATION_ISSUED` (MVD Ministry study visa invitation)
  - `VISA_APPROVED`
  - `ENROLLED`

### 3.3 DocumentService (Ephemeral Presigning Engine)
* Enforces private cloud storage architecture.
* Generates temporary, cryptographically signed S3 URLs with 15-minute Time-To-Live (TTL).
* Prevents unauthorized access or scraping of student passports and NEET scorecards.

### 3.4 EligibilityService (NMC FMGL 2021 Evaluator)
* Evaluates candidate 12th PCB percentage against statutory reservation category thresholds.
* Validates candidate age compliance (17 years completed by December 31).
* Verifies NEET-UG qualifying score validity.
