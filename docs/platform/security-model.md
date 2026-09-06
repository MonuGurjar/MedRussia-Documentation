# MedRussia Platform — Security Architecture & Data Protection

## 1. Security Architecture Overview

MedRussia Platform implements a modern defense-in-depth posture protecting candidate personal data, educational transcripts, and administrative workflows.

---

## 2. Authentication & Credential Storage

### 2.1 Password Hashing with Argon2-cffi
* Passwords submitted during registration or reset are never stored in plaintext.
* The system utilizes **Argon2id** (via `argon2-cffi` and `passlib`), an algorithm designed to resist GPU-based brute-force attacks by utilizing memory-hard computations.
* Every password hash incorporates a unique, cryptographically secure salt.

### 2.2 Stateless JWT Lifecycle & Token Rotation
* **Access Tokens**: Encoded as HMAC-SHA256 JWTs with a short 15-minute expiration (`exp`). They contain standard claims (`sub` with user UUID, `role`, `iat`).
* **Refresh Tokens**: Cryptographically random 64-character tokens with a 30-day expiration. Upon usage, the refresh token is rotated, revoking the previous token to prevent replay attacks.
* **Header Authorization**: Clients supply credentials via the standard `Authorization: Bearer <token>` header.

---

## 3. Authorization & Role-Based Access Control (RBAC)

FastAPI dependencies enforce strict role-based access guards:
* **Public**: University catalog browsing, fee calculations, NMC eligibility checks, and authentication endpoints.
* **Candidate (`student`)**: Submitting admission dossiers, uploading personal KYC documents, inspecting personal milestone status.
* **Staff (`counselor`)**: Viewing assigned student dossiers, updating counselor remarks, assisting candidate inquiries.
* **Administrative (`admin`, `superadmin`)**: Modifying university fee schedules, approving document verifications, managing staff privileges.

---

## 4. Private Cloud Storage & Ephemeral Presigning

To ensure that confidential medical records, marksheets, and passport scans are never exposed publicly:
1. **Private Bucket Security**: Storage buckets are configured to deny all unauthenticated public read/write requests.
2. **Server Authorization**: When a client requests to view or download a document (`GET /api/v1/documents/{id}/signed-url`), FastAPI verifies user ownership or staff privileges.
3. **Cryptographic Presigning**: The server signs a temporary URL using cloud storage credentials with an explicit 15-minute expiration time.
4. **Zero Link Sharing**: Expired links return HTTP 403 Forbidden. Public users cannot guess or scrape document storage URLs.

---

## 5. Audit Logging & Observability

* Inbound requests are tagged with a unique `request_id` via custom ASGI middleware.
* Structured JSON logs generated via `structlog` record the timestamp, HTTP method, path, response status, and duration.
* Sensitive parameters (passwords, tokens, raw file contents) are strictly excluded from logs.
