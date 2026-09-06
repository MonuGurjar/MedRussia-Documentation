# MedRussia — Project Roadmap & Development Milestones

This roadmap outlines the past achievements, active development tasks, and strategic milestones planned for the MedRussia platform.

---

## 1. Completed Milestones (v1.0 Baseline)

- [x] **Platform Architecture Unification**: Established the central FastAPI backend as the canonical Single Source of Truth (SSOT).
- [x] **Standardized Communication Contracts**: Implemented the Response Envelope Pattern and RFC 7807 Problem Details across all endpoints.
- [x] **Stateless Authentication Engine**: Deployed JWT access and refresh token lifecycle with secure rotation and Argon2-cffi password hashing.
- [x] **40+ University Catalog**: Structured verified data models for 40+ recognized Russian medical institutions, complete with fee tiers, accreditation, and hostel parameters.
- [x] **Interactive 6-Year Financial Planner**: Engineered dynamic multi-currency budget calculators (INR, RUB, USD).
- [x] **AI NMC FMGL 2021 Eligibility Engine**: Implemented algorithmic checks for 12th PCB cutoff marks, NEET qualification, and age limits.
- [x] **Private KYC Document Vault**: Configured private cloud object storage with ephemeral, 15-minute presigned signed URLs for confidential student records.
- [x] **5-Stage Admission Dossier State Machine**: Structured end-to-end candidate lifecycle states from application to MVD invitation and enrollment.
- [x] **Multi-Platform Client Sync**: Connected Web (React 19 / Vite) and Android (Kotlin / Jetpack Compose) directly to the central platform API.
- [x] **AI MD Counselor**: Integrated Google Gemini API proxy providing 24/7 conversational advising.

---

## 2. In Progress (Active Development)

- [ ] **Strict User-Scoped Client Caching**: Hardening Android client caching to guarantee strict account isolation during device-level multi-user switching.
- [ ] **Automated End-to-End Test Suite**: Expanding integration test coverage across simulated candidate registration, document upload, and admission submission.
- [ ] **Performance & Bundle Optimization**: Reducing Web client bundle size and fine-tuning Android R8/ProGuard shrinking rules.
- [ ] **Real-time Dossier Event Notifications**: Establishing server-sent events (SSE) for instant milestone status change broadcasts.
- [ ] **Refined Document Previewer**: Native in-app PDF and high-res image viewers for candidate document inspection without browser redirects.

---

## 3. Planned (Next Major Release)

- [ ] **Parental Monitoring Portal**: Dedicated restricted view for parents to track application milestones and verified tuition payment receipts.
- [ ] **Counselor & Staff Management Console**: Role-based web interface for academic counselors to review dossiers, verify marksheets, and issue admission letters.
- [ ] **Multi-Language Support**: Complete localization for Hindi, Russian, and English across Web and Android interfaces.
- [ ] **Integrated Visa & Travel Tracker**: Flight group coordination and airport pickup scheduling for student batches traveling to Moscow, Kazan, and Ufa.

---

## 4. Future Vision

- [ ] **Direct University Fee Escrow**: Secure integration with regulated cross-border banking rails for automated INR-to-RUB university tuition settlements.
- [ ] **NExT / FMGE Academic Prep Module**: Question bank and clinical vignette repository assisting students in preparing for Indian licensing exams during their 6-year MBBS tenure.
- [ ] **Alumni & Senior Mentorship Network**: Verified network connecting enrolled students with Indian doctors practicing after graduating from Russian medical institutions.
