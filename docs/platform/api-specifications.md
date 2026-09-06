# MedRussia Platform — API Specification & Contracts

## 1. OpenAPI 3.1 Standards
The MedRussia Platform API conforms to the **OpenAPI 3.1** specification.
Interactive documentation is available at:
* **Swagger UI**: `/docs`
* **ReDoc**: `/redoc`
* **JSON Schema**: `/openapi.json`

---

## 2. Standardized Communication Envelopes

### 2.1 Success Response (`ResponseEnvelope[T]`)
All successful endpoints wrap their payload within a standard envelope:

```json
{
  "success": true,
  "data": {
    "id": "b4c2e6d8-1234-5678-9abc-def012345678",
    "name": "Bashkir State Medical University",
    "slug": "bashkir",
    "city": "Ufa",
    "tuition_fee_rub": 315000,
    "has_indian_mess": true
  },
  "error": null,
  "meta": {
    "timestamp": "2026-09-01T12:00:00.000Z",
    "request_id": "req-8f4b2c1a",
    "version": "0.1.0"
  }
}
```

### 2.2 RFC 7807 Error Envelope
When an error occurs, the server emits a structured RFC 7807 problem detail response:

```json
{
  "success": false,
  "data": null,
  "error": {
    "type": "about:blank",
    "title": "Validation Error",
    "status": 422,
    "detail": "Invalid input parameter.",
    "code": "VALIDATION_ERROR",
    "invalid_params": [
      {
        "name": "email",
        "reason": "The email address is not valid. It must have exactly one @-sign."
      }
    ],
    "request_id": "req-8f4b2c1a"
  },
  "meta": {
    "timestamp": "2026-09-01T12:00:00.000Z",
    "request_id": "req-8f4b2c1a",
    "version": "0.1.0"
  }
}
```

---

## 3. Core API Endpoints

### 3.1 Authentication & Profile
* `POST /api/v1/auth/register` — Register a new student account.
* `POST /api/v1/auth/login` — Authenticate and receive JWT access & refresh tokens.
* `POST /api/v1/auth/refresh` — Rotate expired access token using valid refresh token.
* `POST /api/v1/auth/logout` — Invalidate current session and revoke tokens.
* `GET  /api/v1/users/me` — Retrieve the authenticated user's profile.

### 3.2 Universities & Catalogs
* `GET  /api/v1/universities` — Paginated list of universities with budget and mess filters.
* `GET  /api/v1/universities/{id}` — In-depth profile, faculties, and 6-year fee tables.
* `POST /api/v1/universities/compare` — Comparative matrix across selected university IDs.

### 3.3 Admissions & Dossiers
* `POST /api/v1/applications` — Submit a new 5-stage admission application.
* `GET  /api/v1/applications/my-dossier` — Retrieve the active candidate's application and milestone status.
* `GET  /api/v1/applications/{id}` — Retrieve detailed application status by dossier ID.

### 3.4 Documents & KYC
* `POST /api/v1/documents/upload` — Multipart upload of marksheets and passports.
* `GET  /api/v1/documents` — List uploaded KYC records for the current user.
* `GET  /api/v1/documents/{id}/signed-url` — Generate an ephemeral 15-minute presigned download URL.

### 3.5 Compliance & Financial Calculations
* `POST /api/v1/eligibility/evaluate` — Algorithmic evaluation of NMC FMGL 2021 compliance.
* `POST /api/v1/calculator/estimate` — Calculate 6-year itemized budget in INR, RUB, or USD.

### 3.6 AI Advisory
* `POST /api/v1/ai/counselor` — Conversational query to the 24/7 AI MD advising engine.
