# MedRussia — System Architecture & Design Overview

## 1. Architectural Principles

MedRussia is designed around four foundational principles:
1. **Single Source of Truth (SSOT)**: The central FastAPI backend owns all domain models, calculations, eligibility evaluations, and application states. Clients (Web and Mobile) act as presentation and user-interaction layers, never implementing diverging business rules.
2. **Stateless & Scalable API Gateway**: All interactions occur over stateless HTTPS REST endpoints authenticated via JSON Web Tokens (JWT). The backend holds no in-memory session state, allowing seamless horizontal scaling.
3. **Defense in Depth & Private Storage**: Student identity records, marksheets, and passport copies are treated as sensitive KYC data. Storage buckets are completely private; access is granted strictly through ephemeral, cryptographically signed URLs with short lifespans (15 minutes).
4. **Standardized Communication Contracts**: All responses adhere to the **Response Envelope Pattern**, and all error responses implement the **IETF RFC 7807 Problem Details** specification.

---

## 2. High-Level Tier Diagram

```
+-----------------------------------------------------------------------------------+
|                                  CLIENT TIER                                      |
|                                                                                   |
|   +------------------------------------+   +----------------------------------+   |
|   |         MedRussia Web              |   |        MedRussia Android         |   |
|   |   (React 19 / TypeScript / Vite)   |   |   (Kotlin / Jetpack Compose)     |   |
|   +-----------------+------------------+   +-----------------+----------------+   |
+---------------------|----------------------------------------|--------------------+
                      |                                        |
                      | HTTPS / REST (JSON)                    | HTTPS / Ktor (JSON)
                      | Bearer JWT Authentication              | Bearer JWT Authentication
                      v                                        v
+-----------------------------------------------------------------------------------+
|                             CENTRAL PLATFORM API                                  |
|                                                                                   |
|   +---------------------------------------------------------------------------+   |
|   |                       FastAPI Asynchronous Gateway                        |   |
|   |  - Route Handling & Middleware Pipeline (CORS, Request IDs, Auth Guards)  |   |
|   |  - Pydantic v2 Request Validation & Response Serialization                |   |
|   +-------------------------------------+-------------------------------------+   |
|                                         |                                         |
|   +-------------------------------------+-------------------------------------+   |
|   |                         Domain Services Layer                             |   |
|   |  - Authentication & Token Service     - University & Fee Engine           |   |
|   |  - Application Dossier Service        - Document Vault & Presigning       |   |
|   |  - AI MD Advisory Proxy               - NMC FMGL 2021 Evaluator           |   |
|   +-------------------------------------+-------------------------------------+   |
+-----------------------------------------|-----------------------------------------+
                                          |
                      +-------------------+-------------------+
                      |                                       |
                      v                                       v
+-----------------------------------+   +-----------------------------------+
|          DATA ACCESS              |   |          CLOUD STORAGE            |
|                                   |   |                                   |
|  +-----------------------------+  |   |  +-----------------------------+  |
|  |     SQLAlchemy 2.0 Async    |  |   |  |     Private S3 Bucket       |  |
|  |     PostgreSQL (asyncpg)    |  |   |  |   (Zero Public Access)      |  |
|  +-----------------------------+  |   |  +-----------------------------+  |
|                                   |   |  - Server-generated signed URLs   |
|  - ACID Transactions              |   |  - Ephemeral 15-minute access     |
|  - Foreign Key Constraints        |   |  - Automated KYC segregation      |
|  - Alembic Versioned Migrations   |   +-----------------------------------+
+-----------------------------------+
```

---

## 3. Communication Contracts

### 3.1 Standard Response Envelope
All API endpoints return a standardized outer envelope to eliminate client-side parsing ambiguities:

```json
{
  "success": true,
  "data": {
    "id": "c7a8b123-4567-890a-bcde-f0123456789a",
    "dossier_number": "RU-2026-0042",
    "status": "Ministry Invitation Letter Issued",
    "current_step": 3
  },
  "error": null,
  "meta": {
    "timestamp": "2026-09-01T12:00:00Z",
    "request_id": "req-9a1b2c3d-4e5f",
    "version": "0.1.0"
  }
}
```

### 3.2 RFC 7807 Problem Details
When an error occurs, the server responds with an RFC 7807 compliant error envelope:

```json
{
  "success": false,
  "data": null,
  "error": {
    "type": "about:blank",
    "title": "Unprocessable Entity",
    "status": 422,
    "detail": "Input validation failed on 1 field.",
    "code": "VALIDATION_ERROR",
    "invalid_params": [
      {
        "name": "pcb_percentage",
        "reason": "Input should be greater than or equal to 50"
      }
    ],
    "request_id": "req-9a1b2c3d-4e5f"
  },
  "meta": {
    "timestamp": "2026-09-01T12:00:00Z",
    "request_id": "req-9a1b2c3d-4e5f",
    "version": "0.1.0"
  }
}
```

---

## 4. Document Vault & Ephemeral Presigning Flow

To protect sensitive applicant documents (Passports, NEET scorecards, 12th marksheets), MedRussia implements a private cloud presigning architecture:

1. **User requests document preview**: Client requests `GET /api/v1/documents/{id}/signed-url`.
2. **Authentication & Authorization**: FastAPI verifies the user's JWT and verifies ownership (or staff permissions).
3. **Signature Generation**: The server interacts with the cloud storage provider to compute an HMAC-SHA256 presigned URL with a 15-minute Time-To-Live (TTL).
4. **Direct Secure Download**: The client receives the signed URL and renders the document directly in the browser or app sandbox. The document cannot be accessed without the cryptographic signature or after expiry.

---

## 5. Security & Isolation Model

* **Stateless Token Management**: Client stores access tokens in secure memory or protected preferences. Refresh tokens rotate upon renewal.
* **Separation of Concerns**: UI components handle user events and reactive rendering; ViewModels and Repositories handle API interactions; FastAPI enforces permissions, data persistence, and domain validation.
