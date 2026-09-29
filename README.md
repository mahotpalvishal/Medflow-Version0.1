# PRD: MedFlow Hospital HIS

**Version:** 1.0  
**Status:** Draft  
**Owner:** Product Team  
**Last Updated:** October 26, 2023

---
## Medflow - HIS Components 
```
┌──────────────────────────────────────────────────────────────────────────────┐
│                              ACCESS LAYER                                    │
│                      ┌──────────────┐   ┌──────────────┐   ┌──────────────┐  │
│                      │ Doctor /     │   │ Nurse /      │   │ Admin /      │  │
│                      │ Clinician    │   │ Staff App    │   │ Back-office  │  │
│                      └──────┬───────┘   └──────┬───────┘   └──────┬───────┘  │
└─────────────────────────────┼──────────────────┼──────────────────┼──────────┘
                              │                  │                  │
                              ┴────────┬─────────┴──────────────────┘
                                       │  (REST / GraphQL / HL7 FHIR)
┌──────────────────────────────────────▼───────────────────────────────────────┐
│                        CORE HOSPITAL OS (HIS)                                │
│  ┌────────────┬────────────┬─────────────────────────────────────────────────| 
│  │ Patient    │ Scheduling │                                                 |
│  │ Registry   │ (Appts)    │                                                 |
│  └────────────┴────────────┴                                                |
│  ┌────────────┬────────────┬                                                 |
│  │ Users &    │ Audit  &   │                                                 |
│  │ RBAC       │ Compliance │                                                 |
│  └────────────┴────────────┴                                                 |
|──────────────────────────────────────────────────────────────────────────────|
```

---

## Frontend 
```
                         MedFlow Web Platform
                                  │
                ┌─────────────────┼─────────────────┐
                │                 │                 │
                ▼                 ▼                 ▼
          Admin Portal      Clinical Portal    Operations Portal
                │                 │                 │
        ┌───────┴───────┐   ┌─────┴──────┐   ┌──────┴────────┐
        │               │   │            │   │               │
     Hospital         Users Doctor     Nurse Reception     Billing
     Setup            RBAC  │           │       │           │
     Master Data           EMR         Ward   Scheduling
                            │
                         Orders
                            │
                       Clinical Docs
```
---

### Docker Ports
```
┌─────────────────────────────────────────────────────┐
│                    Docker Network                    │
│                                                      │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────┐ │
│  │  Angular     │──▶│  Spring Boot │──▶│ Postgres │ │
│  │  (Nginx)     │   │  (HIS API)   │   │  (DB)    │ │
│  │  Port 80     │   │  Port 8080   │   │ Port5432 │ │
│  └──────────────┘   └──────┬───────┘   └──────────┘ │
│                            │                         │
│                            ▼                         │
│                     ┌──────────────┐                 │
│                     │    Redis     │                 │
│                     │  (cache)     │                 │
│                     └──────────────┘                 │
└─────────────────────────────────────────────────────┘
```
---

## 1. Executive Summary
MedFlow is a comprehensive Hospital Information System (HIS) designed to digitize and streamline clinical, operational, and administrative workflows. The system follows a modular architecture centered around a "Core Hospital OS," serving three distinct user portals (Admin, Clinical, Operations) via a unified web platform and mobile access.

---

## 2. Goals & Objectives
- **Unified Patient Record:** Create a single source of truth for patient data across OP, IP, and ER encounters.
- **Clinical Efficiency:** Reduce administrative burden on doctors and nurses through CPOE and digital documentation.
- **Operational Visibility:** Provide real-time data on bed occupancy, scheduling, and billing.
- **Compliance:** Ensure strict adherence to healthcare data privacy and audit standards.

---

## 3. Target Users (Personas)
1. **Clinicians (Doctors):** Need quick access to patient history, order entry (CPOE), and clinical documentation.
2. **Nursing Staff:** Need ward management, medication administration, and vitals tracking.
3. **Front Office/Reception:** Need appointment scheduling, patient registration, and billing.
4. **Hospital Administrators:** Need master data setup, user management (RBAC), and analytics.

---


---

## 🏥 Clinical Services (Top Row)

### 1. Patient Registry

**The single source of truth for patient identity.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Maintains the Master Patient Index (MPI) — one record per real person, across the hospital. |
| **Key Functions** | • Register new patients (walk-in, referral, pre-registration)<br>• Generate unique MRN (Medical Record Number)<br>• Store demographics (name, DOB, gender, address, contacts, insurance)<br>• Patient search & duplicate detection<br>• Merge/unmerge duplicate records<br>• Maintain patient consent & preferred language |
| **Who Uses It** | Registration desk, nurses, doctors, billing. |
| **Talks To** | Encounters, Scheduling, Orders, Master Data, Audit. |
| **Example Events** | `PatientRegistered`, `PatientMerged`, `DemographicsUpdated`. |

---

### 2. Scheduling (Appointments)

**Manages time, people, and rooms.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Books and tracks appointments across clinics, departments, and resources. |
| **Key Functions** | • Provider & resource calendars (doctors, rooms, equipment)<br>• Appointment booking, rescheduling, cancellation<br>• Waitlists & overbooking rules<br>• Recurring appointments<br>• Reminders (SMS/email) via Notification Engine<br>• No-show tracking |
| **Who Uses It** | Front desk, call center, patients (self-service), doctors. |
| **Talks To** | Patient Registry, Encounters, Notification Engine, Master Data. |
| **Example Events** | `AppointmentBooked`, `AppointmentCancelled`, `NoShowRecorded`. |

---

### 3. Encounters (OP / IP / ER)

**The clinical "container" for every interaction between patient and hospital.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Represents any episode of care — outpatient visit, inpatient admission, or ER visit. |
| **Key Functions** | • Create OP / IP / ER encounters<br>• ADT — Admit, Discharge, Transfer<br>• Bed & ward assignment<br>• Length-of-stay tracking<br>• Link orders, notes, results to the encounter<br>• Discharge summary & follow-up |
| **Who Uses It** | Admission desk, nurses, doctors, bed management. |
| **Talks To** | Patient Registry, Orders, Clinical Docs, Scheduling, Reporting. |
| **Example Events** | `EncounterStarted`, `PatientAdmitted`, `PatientTransferred`, `PatientDischarged`. |

---

### 4. Orders (CPOE)

**Computerized Provider Order Entry — where doctors place clinical orders.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Captures, validates, routes, and tracks all clinical orders. |
| **Key Functions** | • Order entry: medications, labs, imaging, referrals, procedures<br>• Order sets / protocols (e.g., "Chest Pain Panel")<br>• Drug-drug & drug-allergy interaction checks<br>• Dose validation (weight/age/renal)<br>• Route orders to pharmacy, lab, radiology<br>• Order status lifecycle (placed → verified → in-progress → completed → cancelled) |
| **Who Uses It** | Doctors, nurse practitioners, pharmacists, lab & radiology staff. |
| **Talks To** | Encounters, Patient Registry, Master Data, Clinical Docs, Notification, external LIS/RIS. |
| **Example Events** | `OrderPlaced`, `OrderVerified`, `OrderCancelled`, `OrderCompleted`. |

---

### 5. Clinical Docs

**Where clinical notes and documents live.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Stores, versions, and manages all clinical documentation. |
| **Key Functions** | • Progress notes, H&P, discharge summaries, consults<br>• Structured templates + free text<br>• Dictation / voice-to-text integration<br>• Attachments (PDFs, scanned docs, images)<br>• Versioning, amendments, addenda<br>• Electronic signature & co-sign<br>• Note locking after sign-off |
| **Who Uses It** | Doctors, nurses, therapists, medical records staff. |
| **Talks To** | Encounters, Orders, Audit, File Storage, Reporting. |
| **Example Events** | `NoteCreated`, `NoteSigned`, `NoteAmended`. |

---

## ⚙️ Platform Services (Bottom Row)

### 6. Users & RBAC

**Identity, authentication, and authorization for the entire HIS.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Controls who can log in and what they can do. |
| **Key Functions** | • User accounts (staff, contractors, admins)<br>• Authentication (password, MFA, SSO via OIDC/SAML)<br>• Role-Based Access Control (roles: Doctor, Nurse, Admin, Pharmacist…)<br>• Fine-grained permissions (e.g., "can prescribe controlled substances")<br>• Session management & token issuance<br>• Break-glass / emergency access |
| **Who Uses It** | Every service (for auth checks), IT admins. |
| **Talks To** | Audit, all services (as a dependency). |
| **Example Events** | `UserCreated`, `RoleAssigned`, `LoginSucceeded`, `AccessDenied`. |

---

### 7. Master Data

**The hospital's reference-data dictionary.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Central catalog of all coded/reference data used across the HIS. |
| **Key Functions** | • Drug formulary (with strengths, forms, routes)<br>• Diagnosis codes (ICD-10, SNOMED CT)<br>• Procedure codes (CPT, ICD-10-PCS)<br>• Lab test catalog (LOINC)<br>• Locations: buildings, wards, rooms, beds<br>• Departments, clinics, providers<br>• Units of measure, frequencies, routes |
| **Who Uses It** | Every clinical & platform service (for lookups & validation). |
| **Talks To** | Terminology Service (if separate), Audit. |
| **Example Events** | `DrugAdded`, `CodeUpdated`, `LocationDeactivated`. |

---

### 8. Audit & Compliance

**The immutable record of everything that happens.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Captures every access and change for legal, regulatory, and security purposes. |
| **Key Functions** | • Append-only audit log (who / what / when / where / why)<br>• PHI access tracking (HIPAA §164.312)<br>• GDPR / data-subject request support<br>• Tamper-evident storage (hash chains, WORM)<br>• Retention & legal-hold policies<br>• Compliance reports & breach detection<br>• Break-glass review |
| **Who Uses It** | Compliance officers, security team, auditors, legal. |
| **Talks To** | Receives events from **every** service. |
| **Example Events** | `PHIAccessed`, `RecordModified`, `ExportPerformed`, `BreakGlassUsed`. |

---

### 9. Notification Engine

**Delivers the right message to the right person at the right time.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Central hub for all outbound and in-app communications. |
| **Key Functions** | • Multi-channel: email, SMS, push, in-app, pager<br>• Templating & localization<br>• User preferences & quiet hours<br>• Escalation rules (e.g., critical lab → doctor → backup doctor)<br>• Delivery tracking & retries<br>• Batch vs. real-time notifications |
| **Who Uses It** | Scheduling (reminders), Orders (critical results), Encounters (admission alerts), Admins. |
| **Talks To** | Users & RBAC (recipients), Master Data (templates), Audit. |
| **Example Events** | `NotificationQueued`, `NotificationSent`, `NotificationFailed`. |

---

### 10. Reporting & Analytics

**Turns operational data into insight.**

| Aspect | Details |
|--------|---------|
| **Purpose** | Provides dashboards, operational reports, and analytical datasets. |
| **Key Functions** | • Operational reports (census, bed occupancy, OR utilization)<br>• Clinical KPIs (readmissions, LOS, mortality)<br>• Financial reports (charges, revenue cycle)<br>• Quality measures (HEDIS, MIPS, core measures)<br>• Scheduled & ad-hoc reports<br>• Data warehouse / lake feeds<br>• Self-service BI for department heads |
| **Who Uses It** | Executives, department heads, quality team, finance, analysts. |
| **Talks To** | All clinical services (read-only), Data Layer (warehouse/lake), Audit. |
| **Example Events** | `ReportGenerated`, `DashboardRefreshed`, `ExportRequested`. |

---

## 🔗 How They Work Together — Example Flow
**A patient arrives with chest pain:**
- Patient Registry → Patient found / registered (MRN issued)
- Scheduling → ER slot booked (or walk-in)
- Encounters → ER encounter created
- Users & RBAC → Doctor authenticated & authorized
- Orders (CPOE) → ECG, troponin, aspirin ordered
- Master Data → Validates drug & lab codes
- Notification Engine → Critical troponin result → alerts cardiologist
- Clinical Docs → ER note written & signed
- Audit & Compliance → Every access & order logged
- Reporting → ER volume + door-to-balloon time updated

## 11. User Stories (High Level)

| ID | As a... | I want to... | So that... |
| :--- | :--- | :--- | :--- |
| **US-01** | Doctor | View a patient's unified timeline (OP/IP/ER) | I can make informed clinical decisions quickly. |
| **US-02** | Nurse | Record vitals and administer meds digitally | We eliminate paper charts and reduce errors. |
| **US-03** | Reception | Schedule an appointment and check insurance | The patient flow is smooth and billing is accurate. |
| **US-04** | Admin | Create a new user and assign a "Doctor" role | The new hire has appropriate system access. |
| **US-05** | Billing | Generate an invoice based on completed encounters | The hospital captures revenue for services rendered. |

---
