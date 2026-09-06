# MedRussia — Security Philosophy & Compliance Architecture

## 1. Security Philosophy

At MedRussia, security and privacy are core architectural tenets, not retrospective additions. Because the platform processes sensitive personal identity records, academic credentials, and international travel documentation, we maintain a strict defense-in-depth posture across all systems.

---

## 2. Core Security Pillars

### 2.1 Private Production Source Code
* Production implementation logic, backend deployment scripts, and internal orchestrations are maintained strictly in private enterprise repositories.
* Public repositories serve exclusively for architectural showcases, documentation, and interface references. They do not contain production code or internal business secrets.

### 2.2 Strict Credential & Secret Management
* **Zero Committed Secrets**: API keys, database connection strings, signing keystores, and third-party tokens are strictly forbidden from version control.
* **Environment-Driven Configuration**: Production secrets are injected at runtime via isolated cloud secrets managers and protected environment variables.
* `.gitignore` rules across all repositories strictly enforce exclusion of `.env`, `.pem`, `.jks`, and credential files.

### 2.3 Ephemeral, Authenticated Document Access
* **No Public Storage Buckets**: Student marksheets, passport pages, and medical certifications are stored in completely private cloud object storage.
* **Temporary Presigned URLs**: Access to any uploaded file requires an authenticated API call. The backend verifies ownership or staff permissions, then returns an ephemerally signed URL with a strictly enforced 15-minute Time-To-Live (TTL).
* **Direct Sandbox Rendering**: Clients render documents inside private sandboxes without exposing persistent public URLs.

### 2.4 Cryptographic Authentication & Token Rotation
* **Argon2-cffi Password Hashing**: Passwords are never stored in plaintext. They are salted and hashed using memory-hard Argon2 algorithms to protect against brute-force and rainbow table attacks.
* **Stateless JWTs with Rotation**: Authentication relies on short-lived JSON Web Tokens (JWT) combined with secure refresh token rotation. Compromised access tokens expire automatically within minutes.

### 2.5 Role-Based Access Control (RBAC)
* Permissions are evaluated strictly at the API gateway layer using dependency injection guards.
* Capabilities are partitioned across defined roles:
  - `student`: Can submit applications, manage personal documents, and track personal dossiers.
  - `counselor`: Can view assigned student applications and communicate via counseling desks.
  - `admin`: Can update university fee parameters and verify submitted documents.
  - `superadmin`: Can manage staff accounts, roles, and platform audit logs.

### 2.6 Auditability & Observability
* Every HTTP request is assigned a unique UUID request correlation ID upon entry.
* Structured JSON logs track operations without recording sensitive payloads (such as passwords or raw tokens), ensuring full auditability while respecting user privacy.

---

## 3. Reporting a Vulnerability

If you discover a potential security vulnerability within the MedRussia platform or documentation, please report it responsibly by contacting our security team at:
📧 **security@medrussia.com** *(placeholder for public security desk)*

Please allow reasonable time for remediation before public disclosure. We appreciate the contributions of ethical security researchers.
