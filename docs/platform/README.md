<div align="center">

  <img src="assets/images/logo.png" alt="MedRussia Platform Logo" width="180" />

  # MedRussia Platform

  ### Centralized RESTful API & Core Business Authority

  [![Python](https://img.shields.io/badge/Python-3.11+-3776AB.svg?style=flat-square&logo=python&logoColor=white)](https://python.org)
  [![FastAPI](https://img.shields.io/badge/Framework-FastAPI%200.115+-009688.svg?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
  [![SQLAlchemy](https://img.shields.io/badge/ORM-SQLAlchemy%202.0%20(Async)-D71F00.svg?style=flat-square&logo=sqlalchemy&logoColor=white)](https://www.sqlalchemy.org)
  [![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL%2015+-336791.svg?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org)
  [![Docker](https://img.shields.io/badge/Deployment-Docker%20Container-2496ED.svg?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com)
  [![License](https://img.shields.io/badge/License-MIT-amber.svg?style=flat-square)](LICENSE)

  <p align="center">
    <strong>The single source of truth governing admissions, university catalogs, financial models, encrypted document vaults, and AI advisory services across the MedRussia ecosystem.</strong>
  </p>

</div>

---

## 📌 Overview

**MedRussia Platform** is the canonical backend engine and RESTful API powering the entire MedRussia ecosystem. Built with Python 3.11+ and FastAPI, it serves as the central authority for business logic, statutory compliance validation (such as India's **NMC FMGL Regulations 2021**), multi-currency financial projections, and student admission dossier lifecycles.

> **Notice**: This repository serves as a public architectural and technical showcase. Sensitive environment variables, production deployment keys, and private infrastructure configurations are secured in our private enterprise repository (`medrussia-platform 🔒`).

---

## 🏛️ Architecture & System Design

```
+---------------------------------------------------------------------------------+
|                             CLIENT INVOCATIONS                                  |
|   - MedRussia Web (React 19 / Vite SPA)                                         |
|   - MedRussia Android (Kotlin / Jetpack Compose Native)                         |
+----------------------------------------+----------------------------------------+
                                         | HTTPS (JSON)
                                         | Bearer JWT Authorization
                                         v
+---------------------------------------------------------------------------------+
|                        FASTAPI ASYNCHRONOUS GATEWAY                             |
|                                                                                 |
|   +-------------------------------------------------------------------------+   |
|   | Middleware Pipeline: CORS, Correlation Request IDs, Latency Profiler   |   |
|   +-------------------------------------------------------------------------+   |
|   | Pydantic v2 Serialization, Validation & RFC 7807 Error Handlers         |   |
|   +-------------------------------------------------------------------------+   |
|                                        |                                        |
|   +------------------------------------+------------------------------------+   |
|   |                           Domain Services                               |   |
|   |  - AuthService: Stateless JWT Issuance & Token Rotation                |   |
|   |  - UniversityService: Catalog, Fee Calculations & Comparisons           |   |
|   |  - ApplicationService: 5-Step Dossier State Machine                     |   |
|   |  - DocumentService: Ephemeral S3 Presigning Engine (15-min TTL)         |   |
|   |  - EligibilityService: NMC FMGL 2021 Algorithmic Compliance             |   |
|   |  - AiService: Google Gemini API Advisory Proxy                          |   |
|   +------------------------------------+------------------------------------+   |
+----------------------------------------|----------------------------------------+
                                         |
                     +-------------------+-------------------+
                     |                                       |
                     v                                       v
+-----------------------------------+   +-----------------------------------+
|       SQLAlchemy 2.0 Async        |   |    Private Cloud Storage (S3)     |
|   (PostgreSQL Connection Pool)    |   |   (Encrypted Student KYC Vault)   |
+-----------------------------------+   +-----------------------------------+
```

### Core Architecture Highlights
* **Response Envelope Pattern**: Guarantees consistent responses across all endpoints (`success`, `data`, `error`, `meta`).
* **IETF RFC 7807 Problem Details**: Standardized machine-readable error responses across all HTTP 4xx/5xx status codes.
* **Stateless JWT with Token Rotation**: Short-lived access tokens (15-minute lifespan) paired with single-use refresh token rotation.
* **Ephemeral S3 Presigning**: Private document vault granting temporary, authenticated access via 15-minute presigned URLs without public bucket exposure.
* **Non-Blocking Async Pipeline**: Pure asynchronous execution using `asyncpg` and SQLAlchemy 2.0.

---

## 📦 Domain Endpoints & Core Services

| Module | Route Prefix | Responsibilities |
| :--- | :--- | :--- |
| **Authentication** | `/api/v1/auth` | User registration, login, JWT token refresh, password resets, session status. |
| **User Identity** | `/api/v1/users` | Profile retrieval (`/me`), student updates, avatar uploads, role inspection. |
| **University Catalog**| `/api/v1/universities`| Catalog search, multi-attribute filtering, fee schedules, university comparisons. |
| **Admission Dossiers**| `/api/v1/applications`| 5-stage admission submission, milestone retrieval, dossier number generation. |
| **Document Vault** | `/api/v1/documents` | Secure KYC upload, metadata tracking, temporary 15-min signed URL generation. |
| **Eligibility & Fees**| `/api/v1/eligibility` | Automated NMC FMGL 2021 compliance check, 6-year multi-currency budget models. |
| **AI MD Counselor** | `/api/v1/ai` | Conversational advising proxy powered by Google Gemini API. |

---

## 🛠️ Technology Stack

* **Language**: Python 3.11+
* **Framework**: FastAPI (ASGI)
* **Web Server**: Uvicorn with `uvloop`
* **Data Validation**: Pydantic v2 & Pydantic Settings
* **Database ORM**: SQLAlchemy 2.0 (Async)
* **Database Driver**: `asyncpg` (PostgreSQL async engine)
* **Migrations**: Alembic
* **Security & Auth**: PyJWT, Argon2-cffi, Passlib
* **Logging**: `structlog` (structured JSON output with correlation IDs)
* **HTTP Client**: HTTPX (Async external API requests)
* **Documentation**: Automated OpenAPI 3.1, Swagger UI (`/docs`), ReDoc (`/redoc`)

---

## 🛡️ Security & Privacy Guardrails

* **Zero Committed Secrets**: Secrets and database credentials are strictly injected at runtime via environment variables.
* **Password Hashing**: Memory-hard Argon2-cffi algorithm with cryptographic salting prevents rainbow table attacks.
* **Private Cloud Storage**: Sensitive KYC documents (passports, NEET scorecards) reside in private buckets with zero public URL exposure.
* **Role-Based Access Control (RBAC)**: Enforces least-privilege access across `student`, `counselor`, and `admin` roles.
* **Audit Trail**: Every request carries a unique UUID correlation ID logged in structured JSON format.

---

## 📂 Repository Structure

```
MedRussia-Platform/
│
├── README.md               # Public platform overview & architecture
├── LICENSE                 # MIT License
│
├── docs/
│   ├── api-architecture.md  # Deep dive into async services & DI
│   ├── api-specifications.md# OpenAPI 3.1 and RFC 7807 contracts
│   └── security-model.md    # Authentication, RBAC, and storage security
│
└── assets/
    └── images/              # Branding assets and architecture diagrams
```

---

## 📄 License

This documentation repository is distributed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

<div align="center">
  <sub>Engineered for reliability, security, and scale. © 2026 MedRussia Technologies.</sub>
</div>
