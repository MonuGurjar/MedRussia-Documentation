# MedRussia — Product Feature Specification

## 1. Overview
MedRussia provides a digital bridge connecting Indian medical candidates directly with accredited, government-funded medical institutions in the Russian Federation. Every feature is designed around transparency, compliance with the **National Medical Commission (NMC)**, and end-to-end security.

---

## 2. Core Functional Modules

### 2.1 University Catalog & Explorer
* **Comprehensive Institution Profiles**: Detailed profiles for 40+ leading Russian government medical universities (e.g., Bashkir State Medical University, Kazan Federal University, First Moscow State Medical University).
* **Multi-Attribute Filtering**: Filter universities by:
  - Annual Tuition and Total Budget brackets.
  - Indian Mess availability (on-campus North/South Indian dining).
  - Hostel infrastructure (Standard 2-to-3 sharing vs. Comfort suites).
  - NMC FMGL 2021 statutory compliance.
* **Side-by-Side Comparison**: Compare up to 3 universities simultaneously across 15+ academic and financial parameters.

### 2.2 Interactive 6-Year Cost Calculator
* **Deterministic Financial Forecasting**: Calculates comprehensive 6-year budgets beyond mere tuition.
* **Dynamic Breakdown**:
  - Tuition fees (annual and total).
  - Hostel accommodation.
  - Compulsory medical insurance (DMS) and clinical state registration.
  - Indian mess / food subscription.
  - Annual return airfare estimates and visa renewals.
* **Multi-Currency Conversion**: Live toggling between Indian Rupees (INR), Russian Rubles (RUB), and US Dollars (USD).

### 2.3 AI NMC Eligibility Checker
* **FMGL 2021 Gazette Compliance**: Instant automated assessment verifying:
  - Physics, Chemistry, and Biology (PCB) percentage against category cutoffs (50% for General/UR, 40% for OBC/SC/ST).
  - Age threshold (17 years completed by December 31 of admission year).
  - NEET-UG qualification status and validity (valid for 3 academic years for study abroad).
* **Detailed Compliance Report**: Generates a downloadable advisory summary outlining statutory eligibility.

### 2.4 5-Stage Live Admission Dossier Tracker
* **Step 1: Application Submission & Verification**: Academic qualification screening and document validation.
* **Step 2: University Admission Letter**: Official preliminary seat reservation from the chosen medical faculty.
* **Step 3: MVD Ministry Electronic Invitation**: Official study visa invitation issued by the Ministry of Internal Affairs of the Russian Federation.
* **Step 4: Visa Stamping & Travel Coordination**: Embassy submission, MEA apostille verification, and flight group formation.
* **Step 5: Campus Arrival & University Enrollment**: Airport pickup, hostel room allocation, medical checkup, and biometric registration.

### 2.5 Student Document Vault (KYC & Academic Records)
* **Secure Document Archiving**: Encrypted cloud storage for Passports, 10th/12th Marksheets, NEET Scorecards, Medical Fitness Certificates, and HIV reports.
* **Tamper-Resistant Storage**: Stored in private object storage; viewing and download occur via authenticated 15-minute temporary presigned links.
* **Document Status Lifecycle**: Real-time status tags (`Pending Verification`, `Approved`, `Re-upload Requested`).

### 2.6 AI MD Counselor & Human Advisory Desk
* **AI MD Counselor (Google Gemini Powered)**: 24/7 interactive conversational assistant trained on NMC regulations, curriculum structures, climate adjustments, and licensing preparation.
* **Human Counselor Desk**: Direct channel for candidates and parents to schedule personalized consultations with verified academic coordinators.

---

## 3. Client Experiences

### 3.1 Web Platform (React 19 / Vite)
* Fully responsive layout tailored for desktop, tablet, and mobile browsers.
* Frosted glassmorphism elements, accessible typography, and smooth page transitions.
* Guest exploration mode with non-intrusive authentication modals for sensitive actions.

### 3.2 Native Android App (Kotlin / Jetpack Compose)
* Smooth native 60fps/120fps animations powered by Compose.
* Offline-first token management and local profile caching.
* Hardware back-navigation handling and edge-to-edge system bar integration.

---

## 4. Planned & Future Capabilities

* **Push Notification Service**: Automated SMS and Push updates for visa invitation issuance. *(TODO: Confirm feature timeline)*
* **Integrated Fee Escrow & Forex Payment Gateway**: Direct INR to RUB university fee payments with automated regulatory receipts. *(TODO: Confirm payment partner integration)*
* **Peer Community & Senior Mentorship Network**: Verified student community forums connecting incoming freshmen with senior Indian medical students in Russia. *(TODO: Confirm community moderation roadmap)*
