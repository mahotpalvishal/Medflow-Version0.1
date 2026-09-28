# Medflow-Version0.1 (PRD)

**Version:** 1.0  
**Status:** Project Baseline / Learning Specification  
**Project Type:** Scalable Healthcare / Medical-Imaging Platform  
**Primary Goal:** Project-based learning for Staff/Senior Staff level system design and healthcare-product engineering  
**Primary Languages:** Java, C++, Go, Python  
**Frontend:** Angular  
**Initial Deployment:** Local development with Docker/Compose  
**Target Evolution:** Modular Monolith → Distributed Services → Cloud-Native Platform

---

## Table of Contents

1. [Document Purpose](#1-document-purpose)
2. [Product Vision](#2-product-vision)
3. [Learning Objectives](#3-learning-objectives)
4. [Scope Boundary and Safety](#4-scope-boundary-and-safety)
5. [Target Users / Actors](#5-target-users--actors)
6. [Healthcare Domain Context](#6-healthcare-domain-context)
7. [Core Product Workflows](#7-core-product-workflows)
8. [Product Capabilities](#8-product-capabilities)
9. [Functional Requirements](#9-functional-requirements)
10. [Non-Functional Requirements](#10-non-functional-requirements)
11. [Initial Domain Model](#11-initial-domain-model)
12. [Initial State Models](#12-initial-state-models)
13. [Initial API Surface](#13-initial-api-surface)
14. [Initial Event Catalogue](#14-initial-event-catalogue)
15. [Storage Strategy](#15-storage-strategy)
16. [Architecture Evolution](#16-architecture-evolution)
17. [Phase-Wise Development Plan](#17-phase-wise-development-plan)
18. [AlgoMaster System Design Mapping](#18-algomaster-system-design-mapping)
19. [Language Strategy](#19-language-strategy)
20. [LLD Component Strategy](#20-lld-component-strategy)
21. [Daily Learning and Development Loop](#21-daily-learning-and-development-loop)
22. [First Four Weeks Plan](#22-first-four-weeks-plan)
23. [Repository Structure](#23-repository-structure)
24. [Testing Strategy](#24-testing-strategy)
25. [Observability Strategy](#25-observability-strategy)
26. [Security and Audit Strategy](#26-security-and-audit-strategy)
27. [Healthcare Interoperability Roadmap](#27-healthcare-interoperability-roadmap)
28. [AI Platform Roadmap](#28-ai-platform-roadmap)
29. [Cloud and Kubernetes Roadmap](#29-cloud-and-kubernetes-roadmap)
30. [Performance Engineering](#30-performance-engineering)
31. [Architecture Decision Records](#31-architecture-decision-records)
32. [Definition of Done](#32-definition-of-done)
33. [Milestones and Exit Criteria](#33-milestones-and-exit-criteria)
34. [Portfolio / Interview Outcomes](#34-portfolio--interview-outcomes)
35. [Project Working Model](#35-project-working-model)
36. [Future Enhancements](#36-future-enhancements)
37. [Glossary](#37-glossary)

---

# 1. Document Purpose

This document is the master specification for **MedFlow**, a project-based healthcare engineering initiative designed to combine:

- real-world medical-imaging workflow understanding;
- low-level design (LLD);
- high-level system design (HLD);
- distributed-systems implementation;
- production engineering;
- healthcare domain knowledge;
- AI-service integration;
- cloud-native deployment; and
- frontend development.

The project is intentionally phase-wise. The system should **not** begin as a large set of microservices. It will first be built as a modular monolith, then progressively distributed as real scalability, reliability, and throughput problems are introduced.

The project will use the user's AlgoMaster system-design learning path as the primary theory track. Each system-design concept should be tied to a real MedFlow engineering problem before it is implemented.

---

# 2. Product Vision

## 2.1 Product Name

**MedFlow — Medical Imaging Workflow & AI Platform**

## 2.2 Vision Statement

Build a realistic, scalable software platform that manages the lifecycle of a medical-imaging examination from order creation through scheduling, acquisition, DICOM ingestion, processing, AI analysis, radiologist review, reporting, and interoperability with other healthcare systems.

## 2.3 High-Level Workflow

```text
Doctor / Referring System
           |
           v
    Imaging Order
           |
           v
      Scheduling
           |
           v
   MRI / CT / X-Ray
           |
           v
    DICOM Ingestion
           |
           v
      Image Storage
           |
      +----+----+
      |         |
      v         v
  Processing   AI Analysis
      |         |
      +----+----+
           |
           v
    Radiologist Worklist
           |
           v
         Report
           |
           v
 Hospital / Healthcare Systems
```

## 2.4 Product Philosophy

MedFlow should demonstrate engineering reasoning rather than technology accumulation.

The guiding pattern is:

```text
Problem
  -> Requirements
  -> Domain Model
  -> LLD
  -> HLD
  -> Implementation
  -> Tests
  -> Failure Injection
  -> Measurement
  -> Optimization
  -> Architecture Decision
```

A technology should be introduced because it solves a concrete problem. For example:

```text
Synchronous image processing blocks API requests
            -> asynchronous processing needed

Jobs can take minutes
            -> durable queue needed

Workers can fail
            -> retries + DLQ needed

Messages can be delivered more than once
            -> idempotent consumers needed

Workload grows
            -> horizontally scalable consumers needed
```

---

# 3. Learning Objectives

MedFlow is both a product and a structured engineering laboratory.

## 3.1 System Design Objectives

Learn and implement, where justified:

- requirements analysis;
- API design;
- scalability;
- load balancing;
- caching;
- database design;
- indexing;
- replication;
- partitioning and sharding;
- consistent hashing;
- message queues;
- event-driven architecture;
- asynchronous processing;
- eventual consistency;
- distributed locking;
- optimistic concurrency;
- idempotency;
- retries and backoff;
- dead-letter queues;
- rate limiting;
- fault isolation;
- search;
- object storage;
- observability;
- disaster recovery;
- multi-region architecture; and
- cloud-native deployment.

## 3.2 LLD Objectives

Practice:

- SOLID;
- composition over unnecessary inheritance;
- interfaces and dependency inversion;
- domain modelling;
- state machines;
- design patterns;
- testability;
- concurrency-safe components;
- clean API boundaries; and
- maintainable production code.

Relevant patterns will be evaluated rather than blindly inserted:

- State
- Strategy
- Factory
- Observer
- Builder
- Adapter
- Repository

## 3.3 Healthcare Objectives

Build practical familiarity with:

- MRI workflow;
- DICOM;
- PACS;
- RIS;
- VNA concepts;
- worklists;
- Study / Series / Instance concepts;
- DICOM identifiers and metadata;
- DICOMweb concepts;
- HL7 concepts;
- FHIR resources and interoperability concepts;
- radiology reporting workflow;
- auditability; and
- healthcare-oriented security considerations.

## 3.4 Software Engineering Objectives

Demonstrate meaningful use of:

- C++;
- Java / Spring Boot;
- Go;
- Python;
- PostgreSQL;
- Redis;
- Kafka;
- object storage;
- search infrastructure;
- Docker;
- Kubernetes;
- observability tooling;
- CI/CD; and
- Angular.

---

# 4. Scope Boundary and Safety

MedFlow is an educational and portfolio project.

The implementation should use:

- synthetic patients;
- synthetic study identifiers;
- synthetic imaging data;
- public standards and documentation;
- open-source libraries and tools; and
- simulated scanners/devices.

The project must **not** reproduce proprietary company source code, architecture diagrams, internal protocols, confidential configurations, customer data, or internal interfaces.

The project can use MRI domain knowledge at the conceptual level to improve realism while remaining an independent implementation.

---

# 5. Target Users / Actors

## 5.1 Radiologist

Primary responsibilities:

- view assigned imaging studies;
- prioritize studies;
- inspect study metadata;
- inspect images;
- review AI findings;
- create reports;
- finalize reports; and
- amend reports when required.

## 5.2 Radiology Technician

Responsibilities:

- view scheduled examinations;
- start examinations;
- associate acquisition data with an order;
- monitor acquisition status; and
- handle basic acquisition workflow status.

## 5.3 Referring Physician

Responsibilities:

- create imaging orders;
- view examination status;
- access completed reports; and
- review relevant prior-study information.

## 5.4 Administrator

Responsibilities:

- manage users;
- manage roles;
- manage imaging centers;
- manage modality/scanner configuration;
- inspect system health; and
- access audit information.

## 5.5 Imaging Device / Simulator

Examples:

- MRI;
- CT;
- X-Ray.

For early development, MRI is the primary simulated device.

## 5.6 AI Processing Service

Consumes imaging studies or derived data and produces an AI result associated with the study.

---

# 6. Healthcare Domain Context

MedFlow is deliberately centered around medical imaging because it provides a strong bridge between healthcare-domain knowledge and distributed-system engineering.

## 6.1 MRI Domain Learning Track

Learn enough of the following to understand the software workflow:

```text
MRI Examination
   |
   +-- Protocol
   +-- Sequence
   +-- Acquisition
   +-- Reconstruction
   +-- Image Generation
   +-- DICOM Metadata
   +-- PACS / Archive
```

The goal is not to become an MRI physicist. The goal is to understand how software systems interact with the imaging workflow.

## 6.2 DICOM Domain Track

Key concepts:

- Study;
- Series;
- Instance;
- SOP Instance;
- UID concepts;
- modality;
- acquisition time;
- patient/study relationships;
- metadata;
- image objects; and
- storage/transfer concepts.

## 6.3 PACS / RIS Context

At a conceptual level, understand:

```text
Order / Scheduling
       |
       v
      RIS
       |
       v
 Imaging Device
       |
       v
     DICOM
       |
       v
      PACS
       |
       v
 Radiologist
```

MedFlow does not need to reproduce a commercial PACS or RIS. It uses simplified versions of their workflow concepts for learning.

## 6.4 Interoperability Track

Later phases introduce:

- HL7;
- FHIR;
- DiagnosticReport-like concepts;
- Patient resource concepts;
- Observation concepts; and
- DICOMweb concepts.

---

# 7. Core Product Workflows

## 7.1 Workflow A — Create Imaging Order

```text
Doctor / System
      |
      v
Patient lookup
      |
      v
Imaging Order
      |
      +--> Modality
      +--> Body Part
      +--> Protocol
      +--> Priority
```

Example:

```json
{
  "patientId": "PAT-10001",
  "modality": "MRI",
  "bodyPart": "BRAIN",
  "protocol": "BRAIN_STANDARD",
  "priority": "ROUTINE"
}
```

## 7.2 Workflow B — Scheduling

```text
Order
  |
  v
Scheduling Service
  |
  +--> Scanner availability
  +--> Technician availability
  +--> Time slot
  +--> Location
  +--> Priority
  |
  v
Appointment
```

Scheduling is an intentional future concurrency case.

Example race:

```text
Doctor A ----\
              >---- 10:00 MRI slot
Doctor B ----/
```

Only one valid reservation should be committed.

## 7.3 Workflow C — Imaging Acquisition

```text
Scheduled Order
      |
      v
MRI Simulator
      |
      v
DICOM Instances
```

## 7.4 Workflow D — DICOM Ingestion

```text
MRI Simulator
      |
      v
DICOM Gateway
      |
      v
Validation
      |
      v
Metadata Extraction
      |
      +-----------> PostgreSQL
      |
      +-----------> Object Storage
```

## 7.5 Workflow E — Processing

```text
Study Ready
    |
    v
Processing Job
    |
    v
Queue / Event Bus
    |
    v
Worker
    |
    v
Processed Output
```

## 7.6 Workflow F — AI Analysis

```text
Study
  |
  v
AI Request
  |
  v
Python AI Service
  |
  v
Inference
  |
  v
AI Result
```

The initial implementation may be a deterministic mock model. A real clinical model is out of scope.

## 7.7 Workflow G — Radiologist Worklist

```text
Study Queue
    |
    v
Radiologist Worklist
    |
    +--> Priority
    +--> Status
    +--> Modality
    +--> Assigned User
    +--> Age / SLA metadata
```

## 7.8 Workflow H — Reporting

```text
Study
  |
  v
Report Draft
  |
  v
Review
  |
  v
Final
  |
  v
Amended (optional)
```

## 7.9 Workflow I — Audit

```text
Actor
  |
  v
Action
  |
  v
Audit Event
  |
  v
Audit Store
```

Example audit record:

```text
USER: RAD-123
ACTION: VIEW_STUDY
RESOURCE: STUDY-10001
TIME: 2026-09-28T14:32:10
```

---

# 8. Product Capabilities

The platform will progressively support:

1. Patient management
2. Imaging order management
3. Scheduling
4. Study lifecycle management
5. DICOM ingestion
6. Metadata extraction
7. Large-object image storage
8. Image processing jobs
9. Asynchronous job orchestration
10. AI analysis
11. Radiologist worklists
12. Reporting
13. Report versioning
14. Audit logging
15. Authentication
16. Role-based authorization
17. Search
18. Observability
19. Healthcare interoperability
20. Cloud-native deployment

---

# 9. Functional Requirements

## FR-001 — Patient Management

Authorized users shall be able to:

- create a patient;
- update a patient;
- retrieve a patient;
- search patients; and
- retrieve a patient's imaging history.

Example:

```http
GET /api/v1/patients/{patientId}
```

## FR-002 — Imaging Orders

The system shall support creation, retrieval, update, and status transitions of imaging orders.

Initial statuses:

```text
CREATED
SCHEDULED
IN_PROGRESS
COMPLETED
REPORTED
CANCELLED
```

## FR-003 — Scheduling

The system shall support:

- available-slot discovery;
- appointment reservation;
- appointment update/cancellation;
- scanner association;
- technician association; and
- concurrency-safe reservation.

## FR-004 — Study Management

The system shall support:

- creation of imaging studies;
- study status tracking;
- linkage to order and patient;
- modality tracking; and
- linkage to DICOM data.

## FR-005 — DICOM Ingestion

The system shall:

- receive DICOM data from the simulator;
- validate supported metadata;
- identify study/series/instance relationships;
- persist metadata; and
- persist binary objects in object storage.

## FR-006 — Object Storage

The system shall maintain a durable mapping between:

```text
Study/Series/Instance metadata
         |
         v
Object Storage Key
```

## FR-007 — Processing Jobs

The system shall support:

- job submission;
- job status;
- job retry;
- worker execution;
- failure tracking; and
- job-result association.

Initial states:

```text
QUEUED
RUNNING
COMPLETED
FAILED
RETRYING
CANCELLED
```

## FR-008 — AI Analysis

The system shall support:

- model selection;
- AI-job creation;
- asynchronous inference;
- result storage;
- result status; and
- model-version tracking.

## FR-009 — Radiologist Worklist

The system shall support:

- retrieval of ready studies;
- priority ordering;
- assignment to radiologists;
- study claiming/lease concepts in later phases; and
- worklist filtering.

## FR-010 — Reporting

The system shall support:

- creating a report draft;
- editing a draft;
- finalization;
- amendment/versioning; and
- report retrieval.

## FR-011 — Audit

The system shall record security-sensitive or workflow-relevant actions, including at minimum:

- user identity;
- action;
- resource;
- timestamp;
- outcome; and
- service/source where relevant.

## FR-012 — Authentication and Authorization

The platform shall support:

- authentication;
- role assignment;
- role-based authorization;
- protected APIs; and
- service-to-service authorization in later phases.

Initial roles:

```text
ADMIN
PHYSICIAN
RADIOLOGIST
TECHNICIAN
```

## FR-013 — Search

The platform shall support searching/filtering by:

- patient;
- study;
- modality;
- date;
- status;
- priority;
- radiologist; and
- location where applicable.

## FR-014 — Notifications (Future)

The platform may later support:

- processing completed notifications;
- AI completed notifications;
- critical workflow alerts; and
- operational alerts.

---

# 10. Non-Functional Requirements

The values below are **engineering targets for this learning project**, not claims about production clinical-system requirements.

## 10.1 MVP Targets

```text
Concurrent users:       ~100
API throughput:         ~100 requests/sec
Availability target:    99.5%
API latency target:     p95 < 500 ms for ordinary CRUD APIs
```

## 10.2 Intermediate Targets

```text
Concurrent users:       ~1,000
API throughput:         ~1,000 requests/sec
Imaging studies/day:    ~10,000
Availability target:    99.9%
```

## 10.3 Advanced Targets

Design toward:

```text
Concurrent users:       10,000+
Studies/day:            100,000+
Processing workers:     horizontally scalable
Selected service SLO:   99.99% target
```

Actual performance must be validated through load tests and measurements.

## 10.4 Reliability

The system should evolve toward:

- graceful degradation;
- timeouts;
- retries;
- backoff;
- idempotency;
- circuit breaking;
- failure isolation;
- health checks;
- recovery procedures; and
- disaster recovery planning.

## 10.5 Security

The platform should support:

- authenticated access;
- least-privilege authorization;
- protected secrets;
- encryption-in-transit concepts;
- secure storage concepts;
- auditability; and
- sensitive-data handling practices.

## 10.6 Maintainability

Code should be:

- modular;
- testable;
- observable;
- documented; and
- understandable by engineers other than the author.

---

# 11. Initial Domain Model

## 11.1 Patient

```text
Patient
- id
- externalId
- firstName
- lastName
- dateOfBirth
- sex (where needed for the educational model)
- contact / demographic metadata as appropriate
- createdAt
- updatedAt
```

## 11.2 ImagingOrder

```text
ImagingOrder
- id
- patientId
- modality
- bodyPart
- protocol
- priority
- status
- orderingProviderId
- requestedAt
- scheduledAt
- createdAt
- updatedAt
```

## 11.3 Appointment

```text
Appointment
- id
- orderId
- scannerId
- technicianId
- locationId
- startTime
- endTime
- status
- version
```

`version` is a candidate field for optimistic locking.

## 11.4 Study

```text
Study
- id
- orderId
- patientId
- modality
- scannerId
- accessionNumber
- studyInstanceUid
- status
- startedAt
- completedAt
```

## 11.5 Series

```text
Series
- id
- studyId
- seriesInstanceUid
- modality
- seriesNumber
- description
```

## 11.6 Instance

```text
Instance
- id
- seriesId
- sopInstanceUid
- storageObjectKey
- contentType
- sizeBytes
- checksum
```

## 11.7 ProcessingJob

```text
ProcessingJob
- id
- studyId
- jobType
- status
- attemptCount
- createdAt
- startedAt
- completedAt
- errorCode
```

## 11.8 AIAnalysis

```text
AIAnalysis
- id
- studyId
- modelName
- modelVersion
- status
- resultLocation
- inferenceDurationMs
- createdAt
- completedAt
```

## 11.9 Report

```text
Report
- id
- studyId
- authorId
- version
- status
- findings
- impression
- recommendations
- createdAt
- updatedAt
- finalizedAt
```

## 11.10 AuditEvent

```text
AuditEvent
- id
- actorId
- action
- resourceType
- resourceId
- timestamp
- outcome
- sourceService
- metadata
```

---

# 12. Initial State Models

## 12.1 Order State Machine

```text
          +-----------+
          |  CREATED  |
          +-----+-----+
                |
                v
         +------+------+
         |  SCHEDULED |
         +------+------+
                |
                v
         +------+------+
         | IN_PROGRESS|
         +------+------+
                |
                v
         +------+------+
         | COMPLETED  |
         +------+------+
                |
                v
         +------+------+
         |  REPORTED  |
         +-------------+

CREATED / SCHEDULED --> CANCELLED
```

## 12.2 Report State Machine

```text
DRAFT
  |
  v
IN_REVIEW
  |
  v
FINAL
  |
  v
AMENDED
```

The LLD will determine whether a simple State pattern or another model best fits the implementation.

## 12.3 Processing Job State Machine

```text
QUEUED
  |
  v
RUNNING
  |
  +--------> COMPLETED
  |
  v
FAILED
  |
  v
RETRYING
  |
  +--------> RUNNING
  |
  +--------> DLQ / TERMINAL_FAILURE
```

---

# 13. Initial API Surface

The API list is deliberately high-level and will be refined during the API-design phase.

## Patient APIs

```http
POST   /api/v1/patients
GET    /api/v1/patients/{patientId}
GET    /api/v1/patients?query=...
PATCH  /api/v1/patients/{patientId}
```

## Order APIs

```http
POST   /api/v1/orders
GET    /api/v1/orders/{orderId}
GET    /api/v1/orders?patientId=...
PATCH  /api/v1/orders/{orderId}
POST   /api/v1/orders/{orderId}/cancel
```

## Scheduling APIs

```http
GET    /api/v1/slots?scannerId=...&date=...
POST   /api/v1/appointments
GET    /api/v1/appointments/{appointmentId}
POST   /api/v1/appointments/{appointmentId}/cancel
```

## Study APIs

```http
GET    /api/v1/studies/{studyId}
GET    /api/v1/patients/{patientId}/studies
POST   /api/v1/studies
```

## Processing APIs

```http
POST   /api/v1/studies/{studyId}/processing-jobs
GET    /api/v1/processing-jobs/{jobId}
POST   /api/v1/processing-jobs/{jobId}/retry
```

## AI APIs

```http
POST   /api/v1/studies/{studyId}/ai-analyses
GET    /api/v1/ai-analyses/{analysisId}
```

## Reporting APIs

```http
POST   /api/v1/studies/{studyId}/reports
GET    /api/v1/reports/{reportId}
PATCH  /api/v1/reports/{reportId}
POST   /api/v1/reports/{reportId}/finalize
POST   /api/v1/reports/{reportId}/amend
```

These endpoints are starting points only. Detailed API contracts, pagination, filtering, idempotency keys, error models, and versioning will be defined during implementation.

---

# 14. Initial Event Catalogue

Events will be introduced when the architecture moves to asynchronous processing.

```text
PatientCreated
PatientUpdated
OrderCreated
OrderScheduled
OrderCancelled
StudyCreated
DicomReceived
StudyReady
ProcessingRequested
ProcessingStarted
ProcessingCompleted
ProcessingFailed
AIRequested
AIStarted
AICompleted
ReportDraftCreated
ReportFinalized
ReportAmended
AuditEventRecorded
```

Each event will eventually define:

- event name;
- event version;
- producer;
- consumer(s);
- key/partition key;
- schema;
- delivery assumptions;
- retry behavior; and
- idempotency strategy.

Example future event envelope:

```json
{
  "eventId": "evt-123",
  "eventType": "StudyReady",
  "eventVersion": 1,
  "occurredAt": "2026-09-28T14:32:10Z",
  "aggregateType": "Study",
  "aggregateId": "STUDY-10001",
  "correlationId": "corr-789",
  "payload": {}
}
```

---

# 15. Storage Strategy

## 15.1 PostgreSQL

Primary relational database for:

- patient metadata;
- orders;
- appointments;
- studies;
- series metadata;
- instance metadata;
- reports;
- processing metadata; and
- application configuration where appropriate.

## 15.2 Object Storage

Use MinIO locally as an S3-compatible object store.

Store:

- DICOM binary objects;
- derived images;
- processing artifacts; and
- AI output artifacts where appropriate.

Example object key:

```text
studies/{studyId}/series/{seriesId}/instances/{instanceId}.dcm
```

## 15.3 Redis

Introduced later for suitable workloads, potentially:

- cached patient/study metadata;
- worklist caching;
- short-lived locks;
- rate limiting;
- temporary coordination state.

Cache usage must be justified per use case.

## 15.4 Search Store

A dedicated search engine may later be introduced for rich worklist and study search.

The project will use search infrastructure only when relational querying becomes an insufficient fit for the learning scenario.

---

# 16. Architecture Evolution

## Architecture 1 — Modular Monolith

```text
                  Angular
                     |
                     v
             +----------------+
             | Spring Boot    |
             |                |
             | Patient        |
             | Order          |
             | Scheduling     |
             | Study          |
             | Reporting      |
             +-------+--------+
                     |
                PostgreSQL
```

### Purpose

- establish domain boundaries;
- learn LLD;
- build clean APIs;
- understand transactions;
- avoid premature distributed complexity.

## Architecture 2 — Add Object Storage

```text
Spring Boot
    |
    +---- PostgreSQL
    |
    +---- MinIO / S3
```

## Architecture 3 — Async Processing

```text
                    Kafka
                      |
             +--------+--------+
             |        |        |
             v        v        v
          Worker    AI Job    Audit
```

## Architecture 4 — Microservices

```text
                  API Gateway
                       |
       +---------------+---------------+
       |               |               |
       v               v               v
   Patient          Order          Scheduling
   Service          Service          Service
       |               |               |
       +---------------+---------------+
                       |
                       v
                     Kafka
                       |
       +---------------+---------------+
       |               |               |
       v               v               v
   Imaging        Processing           AI
   Service          Service          Service
```

## Architecture 5 — Scaled Platform

```text
                Load Balancer
                      |
              API Gateway / Ingress
                      |
              Multiple API Instances
                      |
        +-------------+-------------+
        |                           |
       Redis                    PostgreSQL
        |                           |
        +-------------+-------------+
                      |
                    Kafka
                      |
                Scalable Workers
                      |
        +-------------+-------------+
        |             |             |
   Processing        AI           Audit
```

## Architecture 6 — Cloud Native

```text
                         Internet
                            |
                     Load Balancer
                            |
                         Ingress
                            |
                       Kubernetes
                            |
       +--------------------+--------------------+
       |                    |                    |
       v                    v                    v
   Microservices         Workers                AI
       |                    |                    |
       +--------------------+--------------------+
                            |
        +-------------------+-------------------+
        |                   |                   |
   PostgreSQL            Redis               Kafka
        |
   Object Storage
        |
   Search / Analytics
```

---

# 17. Phase-Wise Development Plan

## Phase 0 — Product and Healthcare Domain Foundation

**Suggested duration:** 1 week

### Learn

- MRI workflow;
- DICOM basics;
- PACS;
- RIS;
- Study / Series / Instance;
- basic HL7 concepts;
- basic FHIR concepts;
- radiology reporting flow.

### Deliverables

- product requirements;
- domain glossary;
- context diagram;
- primary workflows;
- initial domain model.

### System Design Focus

None initially. The focus is understanding the problem space before choosing architecture.

### Exit Criteria

You can clearly explain the journey:

```text
Order -> Schedule -> Acquire -> Ingest -> Store -> Process -> Review -> Report
```

---

## Phase 1 — Requirements and Domain Modelling

**Suggested duration:** 1 week

### Components

- Patient
- ImagingOrder
- Appointment
- Study
- Series
- Instance
- ProcessingJob
- AIAnalysis
- Report
- AuditEvent

### LLD Topics

- SOLID;
- composition;
- interfaces;
- dependency inversion;
- domain entities/value concepts;
- state modelling;
- invariants.

### Deliverables

- class diagrams;
- sequence diagrams;
- state diagrams;
- initial API contracts.

---

## Phase 2 — LLD Core Components

**Suggested duration:** 1–2 weeks

### Components to design

1. Order state machine
2. Appointment scheduler
3. Study lifecycle
4. Processing job lifecycle
5. Report lifecycle
6. Worklist abstraction
7. Audit abstraction

### Patterns to evaluate

- State
- Strategy
- Factory
- Observer
- Builder
- Adapter
- Repository

### Output per component

```text
Problem
Requirements
Constraints
Class model
Sequence diagram
Pattern decision
Alternative considered
Implementation
Unit tests
Concurrency considerations
Interview discussion
```

---

## Phase 3 — Modular Monolith Backend

**Suggested duration:** 2 weeks

### Technology

- Java
- Spring Boot
- PostgreSQL
- Docker
- JUnit
- integration-test framework / containers

### Build

- Patient API;
- Order API;
- Scheduling API;
- Study API;
- Reporting API.

### System Design Topics

- REST;
- HTTP semantics;
- API versioning;
- transactions;
- connection pooling;
- indexes;
- pagination;
- validation;
- error handling.

### Exit Criteria

A developer can run the backend locally, create a patient, create an order, schedule an examination, create a study, and create/finalize a report.

---

## Phase 4 — DICOM and Imaging Pipeline

**Suggested duration:** 2 weeks

### Technology

- C++
- a public/open DICOM implementation/tooling
- MinIO
- Java integration layer

### Build

```text
MRI Simulator
     |
     v
Synthetic DICOM
     |
     v
DICOM Ingestion
     |
     +------> Metadata DB
     |
     +------> Object Storage
```

### Healthcare Topics

- DICOM hierarchy;
- identifiers;
- modality;
- metadata;
- study/series/instance relations;
- ingestion validation.

### Exit Criteria

A synthetic MRI study can be ingested and retrieved with metadata plus a stable object-storage mapping.

---

## Phase 5 — Distributed Processing

**Suggested duration:** 2 weeks

### Technology

- Kafka
- worker services
- Java/Go consumers

### Build

```text
StudyReady
   |
   v
Kafka
   |
   v
Processing Worker
```

### Learn

- producers;
- consumers;
- partitions;
- consumer groups;
- offsets;
- retries;
- delivery semantics;
- dead-letter queues;
- idempotency.

### Exit Criteria

Processing is asynchronous, observable, retryable, and safe against duplicate deliveries.

---

## Phase 6 — Microservices

**Suggested duration:** 2–3 weeks

### Candidate boundaries

- Patient Service
- Order Service
- Scheduling Service
- Imaging Service
- Processing Service
- Reporting Service
- AI Service
- Audit Service

### Language strategy

```text
Java -> core business services
Go   -> high-throughput / infrastructure-oriented services
C++  -> imaging processing
Python -> AI / data processing
```

### Important Principle

Do not extract a service merely because a domain object exists. A service boundary should be justified by:

- ownership;
- scaling needs;
- deployment independence;
- fault isolation;
- data ownership; or
- clear domain boundaries.

---

## Phase 7 — Caching

**Suggested duration:** 1 week

### Technology

Redis

### Candidate workloads

- hot study metadata;
- worklist queries;
- configuration;
- short-lived coordination state.

### Learn

- cache-aside;
- TTL;
- invalidation;
- stale data;
- cache stampede;
- hot keys;
- negative caching considerations.

### Exit Criteria

You can explain exactly what is cached, why, the consistency impact, and the invalidation strategy.

---

## Phase 8 — Search

**Suggested duration:** 1 week

### Build

A search/read model for the radiologist worklist.

### Learn

- indexing;
- inverted indexes;
- denormalization;
- eventual consistency;
- search ingestion from events.

### Example

```text
PostgreSQL
    |
    v
Study Events
    |
    v
Search Projection
    |
    v
Worklist Search
```

---

## Phase 9 — Distributed Consistency

**Suggested duration:** 2 weeks

### Scenarios

#### Optimistic locking

Scheduling update collisions.

#### Idempotency

Duplicate order submissions and duplicate DICOM/job events.

#### Distributed locking

Competing reservation attempts.

#### Consistent hashing

Candidate worker-routing or partitioning use cases.

#### Eventual consistency

Search/worklist projections.

#### CAP trade-offs

Analyze individual MedFlow workflows and decide where strong vs eventual consistency is appropriate.

### Exit Criteria

You can explain the consistency model for each major workflow and why it was selected.

---

## Phase 10 — Reliability and Fault Tolerance

**Suggested duration:** 2 weeks

### Failure cases

- Kafka unavailable;
- PostgreSQL unavailable;
- Redis unavailable;
- AI service timeout;
- object storage unavailable;
- worker crash;
- duplicate event;
- downstream service timeout;
- partial failure.

### Engineering techniques

- timeouts;
- retries;
- exponential backoff;
- circuit breakers;
- bulkheads;
- DLQs;
- idempotency;
- health checks;
- graceful degradation.

### Exit Criteria

The project has a documented failure-mode matrix and tests for the most important failure paths.

---

## Phase 11 — Observability

**Suggested duration:** 1 week

### Build

- structured logging;
- metrics;
- distributed tracing;
- dashboards.

### Core metrics

```text
API latency
API error rate
RPS
Kafka lag
Job processing duration
Job failure rate
Database latency
DB connection pool usage
AI inference latency
Object-storage operation failures
```

### Trace Example

```text
Request
  -> API Gateway
  -> Order Service
  -> Kafka
  -> Processing Service
  -> AI Service
```

A correlation/trace ID should be preserved across service boundaries.

---

## Phase 12 — Security and Audit

**Suggested duration:** 1–2 weeks

### Build

- JWT authentication;
- RBAC;
- protected APIs;
- service authentication concept;
- audit trail;
- secret management basics.

### Learn

- least privilege;
- defense in depth;
- data classification;
- secure API design;
- auditability;
- healthcare security/compliance concepts at a high level.

---

## Phase 13 — Healthcare Interoperability

**Suggested duration:** 2 weeks

### Add

- FHIR concepts;
- HL7 integration concepts;
- DICOMweb concepts.

### Example logical mapping

```text
FHIR Patient
    |
    v
MedFlow Patient
```

```text
Imaging Study
    |
    v
DiagnosticReport-like representation
```

### Exit Criteria

You can explain how the internal MedFlow model maps to external healthcare interfaces without coupling the entire domain model to an external standard.

---

## Phase 14 — AI Platform

**Suggested duration:** 2–3 weeks

### Technology

Python plus an inference/runtime stack as appropriate.

### Initial architecture

```text
Study
  |
  v
AI Job
  |
  v
Kafka
  |
  v
Python Inference Service
  |
  v
AI Result
```

### Advanced extensions

- model registry;
- model versioning;
- asynchronous GPU workers;
- batching;
- inference metrics;
- model rollout strategies;
- result provenance.

### Exit Criteria

AI analysis is integrated as an asynchronous service with versioned outputs and clear failure handling.

---

## Phase 15 — Frontend

**Suggested duration:** 2–3 weeks

Frontend development begins after the backend/distributed architecture has reached a useful stable state.

### Radiologist Dashboard

```text
Worklist
   |
   v
Study
   |
   +--> Images
   +--> Metadata
   +--> AI Findings
   +--> Report
```

### Technician Dashboard

```text
Schedule
   |
   v
Examination
   |
   v
Acquisition Status
```

### Admin Dashboard

```text
Users
Services
System Health
Audit
```

---

## Phase 16 — Kubernetes and Cloud

**Suggested duration:** 2–3 weeks

### Introduce

- Docker images;
- Kubernetes deployments;
- services;
- ingress;
- ConfigMaps;
- Secrets;
- readiness/liveness probes;
- horizontal pod autoscaling;
- resource requests/limits;
- rolling updates.

### Exit Criteria

Core services run as containers, can scale horizontally, and expose health/observability information.

---

## Phase 17 — Performance Engineering

**Suggested duration:** 1–2 weeks

### Method

```text
Baseline
   |
   v
Load Test
   |
   v
Measure
   |
   v
Find Bottleneck
   |
   v
Optimize
   |
   v
Re-test
```

### Metrics

- p50 latency;
- p95 latency;
- p99 latency;
- throughput;
- CPU;
- memory;
- database connections;
- Kafka lag;
- queue depth.

### Goal

No optimization without measurement.

---

## Phase 18 — Advanced Distributed Architecture

**Suggested duration:** 2–4 weeks

Potential topics:

- database sharding;
- read replicas;
- multi-region systems;
- active-active vs active-passive;
- failover;
- disaster recovery;
- data replication;
- distributed transactions;
- Saga;
- workflow orchestration;
- CQRS;
- event sourcing where it provides a clear learning value.

These should be added only if they solve a sufficiently meaningful MedFlow problem.

---

# 18. AlgoMaster System Design Mapping

The user's AlgoMaster system-design curriculum serves as the theory sequence. MedFlow is the implementation laboratory.

| System Design Topic | MedFlow Application |
|---|---|
| Requirements | Product requirements, actors, workflows |
| API Design | Patient/order/study/report APIs |
| Database Design | Clinical metadata model |
| Indexing | Patient/study/worklist queries |
| Transactions | Order creation, scheduling |
| Concurrency | Slot reservation, worklist claims |
| Caching | Study metadata, worklist |
| Message Queues | Imaging processing |
| Kafka / Streaming | Workflow events |
| Eventual Consistency | Search/worklist projections |
| Idempotency | Duplicate events/requests |
| Rate Limiting | API and ingestion protection |
| Consistent Hashing | Candidate worker routing / partitioning |
| Partitioning | Large-scale study data |
| Sharding | Advanced metadata scale scenario |
| Object Storage | DICOM binaries |
| Search | Worklist and study discovery |
| Fault Tolerance | Worker/downstream failures |
| Circuit Breakers | Service dependency failures |
| Observability | End-to-end imaging pipeline |
| Multi-region | Future platform architecture |

## Learning Rule

For each AlgoMaster topic:

```text
Learn concept
    ↓
Identify a real MedFlow problem
    ↓
Decide whether the concept applies
    ↓
Design the solution
    ↓
Implement
    ↓
Test
    ↓
Load test / failure test
    ↓
Document trade-offs
```

A topic can remain theory-only when MedFlow has no good use case for it.

---

# 19. Language Strategy

The project should not rewrite all functionality in every language.

Each language has a defined role.

| Language | Intended Role |
|---|---|
| **Java / Spring Boot** | Core business services, APIs, persistence |
| **C++** | DICOM and image-processing / performance-sensitive components |
| **Go** | High-throughput gateways, workers, infrastructure-oriented services |
| **Python** | AI inference, data/analytics workloads |
| **Angular / TypeScript** | Frontend |

## 19.1 Why This Strategy Matters

The portfolio should demonstrate architectural language choice:

> Different components can use different languages because their workload, ecosystem, performance, or maintainability characteristics differ.

Not:

> Four languages were used simply to demonstrate that four languages are known.

---

# 20. LLD Component Strategy

Each major LLD component should have its own focused design exercise.

Recommended component chats:

```text
MedFlow — Patient Service LLD
MedFlow — Order Service LLD
MedFlow — Scheduling LLD
MedFlow — Study Lifecycle LLD
MedFlow — DICOM Parser LLD
MedFlow — Processing Job LLD
MedFlow — Worklist LLD
MedFlow — Reporting LLD
MedFlow — Audit LLD
MedFlow — Kafka/Event LLD
MedFlow — Distributed Locking LLD
MedFlow — AI Service LLD
```

When the user shares an external AlgoMaster language or LLD link for a component, use it to identify:

1. What to revise
2. What to implement
3. Which concepts are relevant
4. Which design patterns actually fit
5. What tests should be written
6. What system-design trade-offs should be documented
7. What Staff-level interview questions should be practiced

---

# 21. Daily Learning and Development Loop

Each working day should approximately contain:

```text
System Design Theory      ~20 min
Healthcare Domain         ~20 min
Implementation            ~60–90 min
Testing                   ~20 min
Architecture Notes        ~10 min
```

This can be adjusted around actual available time.

## Standard Daily Task Template

```text
DAY XX

System Design
-------------
Topic:
AlgoMaster reference:
Concepts to understand:

Healthcare
----------
Topic:
What to learn:

LLD
---
Component:
Classes/interfaces:
Patterns:

Implementation
--------------
Task 1:
Task 2:
Task 3:

Testing
-------
Unit tests:
Integration tests:
Failure tests:

Engineering
-----------
Git commit:
Documentation:

Interview
---------
Question 1:
Question 2:
Question 3:
```

---

# 22. First Four Weeks Plan

## Week 1 — Domain + Product

### Day 1

- Review/freeze PRD v1.0
- Set up repository
- Create documentation skeleton
- Define project glossary

### Day 2

- Study MRI imaging workflow
- Document examination lifecycle
- Define primary personas

### Day 3

- Study DICOM basics
- Study Study / Series / Instance
- Build a DICOM terminology page

### Day 4

- Study MRI → DICOM → PACS workflow
- Map MedFlow concepts to the healthcare workflow

### Day 5

- Design domain entities
- Identify invariants
- Identify aggregate boundaries/candidates

### Day 6

- Create use-case diagrams
- Create major sequence diagrams
- Create state machines

### Day 7

- Review domain design
- Perform architecture walkthrough
- Identify open questions

### Week 1 Deliverables

```text
docs/product/prd.md
docs/healthcare/glossary.md
docs/healthcare/imaging-workflow.md
docs/architecture/context.md
docs/architecture/domain-model.md
```

---

## Week 2 — LLD

### Day 8

Order domain LLD.

### Day 9

Order state-machine implementation.

### Day 10

Scheduling LLD.

### Day 11

Appointment concurrency design.

### Day 12

Study lifecycle LLD.

### Day 13

Processing-job LLD.

### Day 14

LLD review, refactoring, tests, and interview discussion.

### Week 2 Deliverables

- class diagrams;
- sequence diagrams;
- state diagrams;
- LLD implementations for core components;
- unit tests.

---

## Week 3 — Backend

### Day 15

Initialize Spring Boot service/application structure.

### Day 16

Patient API.

### Day 17

Order API.

### Day 18

Scheduling API.

### Day 19

Study API.

### Day 20

Report API.

### Day 21

Integration tests, API review, error handling.

### Week 3 Deliverable

Working modular backend with clean domain and persistence boundaries.

---

## Week 4 — Persistence + Imaging

### Day 22

PostgreSQL schema design.

### Day 23

Indexes and query plans.

### Day 24

Transactions, locking, and consistency.

### Day 25

MinIO/object-storage integration.

### Day 26

C++ DICOM component.

### Day 27

DICOM ingestion pipeline.

### Day 28

End-to-end MVP demonstration.

### Week 4 Deliverable

```text
Patient
 -> Order
 -> Appointment
 -> Study
 -> Synthetic DICOM
 -> Ingestion
 -> Object Storage
 -> Metadata DB
 -> Report
```

---

# 23. Repository Structure

The repository should grow with the architecture.

Initial target:

```text
medflow/
|
+-- docs/
|   +-- product/
|   +-- architecture/
|   +-- healthcare/
|   +-- api/
|   +-- adr/
|   +-- runbooks/
|   +-- interview/
|
+-- services/
|   +-- application/            # Initial modular monolith
|   +-- patient-service/
|   +-- order-service/
|   +-- scheduling-service/
|   +-- imaging-service/
|   +-- processing-service/
|   +-- reporting-service/
|   +-- audit-service/
|   +-- ai-service/
|
+-- cpp/
|   +-- dicom/
|   +-- image-processing/
|
+-- frontend/
|
+-- infrastructure/
|   +-- docker/
|   +-- kubernetes/
|   +-- terraform/
|
+-- tests/
|   +-- integration/
|   +-- load/
|   +-- contract/
|
+-- scripts/
|
+-- .github/
|
+-- README.md
```

Not every directory is required on Day 1.

---

# 24. Testing Strategy

Testing is part of the architecture, not a final activity.

## 24.1 Unit Tests

For:

- domain logic;
- state machines;
- scheduling rules;
- validation;
- retry policies;
- job lifecycle rules.

## 24.2 Integration Tests

For:

- PostgreSQL;
- Kafka;
- object storage;
- Redis;
- service-to-service integration.

## 24.3 Contract Tests

Later, verify API/event contracts between services.

## 24.4 End-to-End Tests

Primary flow:

```text
Patient
 -> Order
 -> Schedule
 -> Study
 -> DICOM Ingestion
 -> Processing
 -> AI
 -> Worklist
 -> Report
```

## 24.5 Failure Tests

Deliberately simulate:

- duplicate event;
- timeout;
- dependency failure;
- worker restart;
- retry exhaustion;
- partial state transitions.

## 24.6 Load Tests

Progressively test:

```text
100 users
1,000 users
10,000 users
```

where realistic for the component being tested.

---

# 25. Observability Strategy

## Logs

Use structured logs containing fields such as:

```text
service
requestId
traceId
studyId
jobId
userId
operation
status
latencyMs
```

## Metrics

Minimum metrics:

```text
http_requests_total
http_request_latency
http_error_rate
kafka_consumer_lag
job_queue_depth
job_duration
job_failures
ai_inference_duration
postgres_query_latency
postgres_connection_pool_usage
object_storage_failures
```

## Tracing

Trace the end-to-end journey:

```text
API Request
  -> Order Service
  -> Kafka
  -> Imaging Service
  -> Processing Worker
  -> AI Service
```

## Operational Dashboards

Eventually provide separate views for:

- API health;
- processing health;
- Kafka/queue health;
- database health;
- AI service health; and
- infrastructure health.

---

# 26. Security and Audit Strategy

Security should evolve with the architecture.

## Authentication

Initial choice:

- JWT-based authentication for user APIs.

## Authorization

RBAC:

```text
ADMIN
PHYSICIAN
RADIOLOGIST
TECHNICIAN
```

## Authorization Examples

```text
PHYSICIAN -> create imaging order
RADIOLOGIST -> review/finalize report
TECHNICIAN -> update acquisition status
ADMIN -> manage configuration/users
```

## Service-to-Service Security

Later phases should investigate:

- service identity;
- credential management;
- token propagation;
- least privilege.

## Audit

Audit important operations:

```text
Who?
What?
When?
Which resource?
Success/failure?
Which service?
```

---

# 27. Healthcare Interoperability Roadmap

The interoperability phase should teach concepts without coupling the whole product to a single external standard.

## 27.1 FHIR

Study:

- Patient;
- Observation;
- DiagnosticReport;
- resource relationships;
- identifiers;
- references.

Potential mapping:

```text
MedFlow Patient
       <->
FHIR Patient representation
```

```text
MedFlow Report
       <->
DiagnosticReport-like representation
```

## 27.2 HL7

Learn conceptually:

- message-based healthcare integration;
- event-driven communication;
- interface-engine style architectures;
- transformation/normalization.

## 27.3 DICOMweb

Later investigate web-based access and imaging interoperability concepts.

---

# 28. AI Platform Roadmap

AI should be treated as another distributed subsystem, not simply a Python endpoint.

## Initial AI Service

```text
API
 |
 v
AI Job
 |
 v
Kafka
 |
 v
Python Inference Service
 |
 v
AI Result
```

## Advanced AI Architecture

```text
                   Model Registry
                         |
                         v
Study -> Queue -> GPU Worker -> Result Store
                         |
                         v
                  Model Metrics
```

## Engineering Topics

- asynchronous inference;
- batching;
- model versioning;
- model rollout;
- result provenance;
- inference latency;
- resource utilization;
- failure/retry behavior.

Clinical decisions are outside project scope.

---

# 29. Cloud and Kubernetes Roadmap

The cloud phase occurs only after the distributed backend is stable.

## Containers

Every independently deployable service gets a Docker image.

## Kubernetes

Learn and implement:

- Deployment;
- Service;
- Ingress;
- ConfigMap;
- Secret;
- readiness probe;
- liveness probe;
- Horizontal Pod Autoscaler;
- resource requests/limits;
- rolling update.

## CI/CD

Pipeline should eventually perform:

```text
Commit
  |
  v
Build
  |
  v
Unit Tests
  |
  v
Integration Tests
  |
  v
Container Build
  |
  v
Security / Quality Checks
  |
  v
Deploy
```

---

# 30. Performance Engineering

Performance work must be measurement-driven.

## Baseline

Establish:

- throughput;
- p50/p95/p99 latency;
- CPU;
- memory;
- database latency;
- queue depth;
- Kafka lag.

## Typical Bottleneck Investigation

```text
High API latency
      |
      +--> CPU?
      +--> DB?
      +--> Network?
      +--> Lock contention?
      +--> Downstream service?
      +--> Cache miss?
```

## Performance Topics

- connection pooling;
- batching;
- pagination;
- database indexing;
- query tuning;
- caching;
- asynchronous processing;
- worker parallelism;
- backpressure;
- data partitioning.

---

# 31. Architecture Decision Records

Maintain an ADR directory.

Recommended initial ADRs:

```text
ADR-001-modular-monolith.md
ADR-002-postgresql.md
ADR-003-object-storage.md
ADR-004-dicom-ingestion.md
ADR-005-kafka.md
ADR-006-service-boundaries.md
ADR-007-idempotency.md
ADR-008-redis.md
ADR-009-search.md
ADR-010-report-versioning.md
ADR-011-ai-service-boundary.md
ADR-012-observability.md
ADR-013-security-model.md
ADR-014-kubernetes.md
```

## ADR Template

```text
# ADR-XXX: <Decision Title>

## Context

What problem exists?

## Requirements / Constraints

What must be true?

## Options Considered

Option A
Option B
Option C

## Decision

What was selected?

## Reasoning

Why?

## Trade-offs

What becomes better/worse?

## Consequences

What changes in the architecture?

## Revisit Conditions

When should this decision be reconsidered?
```

---

# 32. Definition of Donepdf or .

A meaningful feature is complete only when the following are addressed:

```text
Requirement
   |
   v
LLD
   |
   v
Implementation
   |
   v
Unit Tests
   |
   v
Integration Tests
   |
   v
Failure Cases
   |
   v
Observability
   |
   v
Documentation
   |
   v
ADR / Design Rationalepdf or .
```

The exact level of rigor depends on phase, but the mindset should remain consistent.

---

# 33. Milestones and Exit Criteria

## Milestone 1 — Domain Understanding

Can explain:

- patient;
- order;
- appointment;
- study;
- series;
- instance;
- DICOM;
- PACS;
- MRI workflow.

## Milestone 2 — LLD Foundation

Can design core MedFlow components using clean object-oriented design and explain pattern choices.

## Milestone 3 — MVP Backend

Can execute the full order-to-report workflow through REST APIs.
pdf or .
## Milestone 4 — Imaging Integration

Can ingest synthetic DICOM data and persist metadata plus binary objects.

## Milestone 5 — Distributed Processing

Can process studies asynchronously through a durable event/queue architecture.

## Milestone 6 — Service Decomposition

Can defend service boundaries and data ownership.

## Milestone 7 — Production Reliability

Can demonstrate retries, idempotency, timeout handling, circuit breaking, and DLQs.

## Milestone 8 — Observable Platform

Can trace and measure the major workflows.

## Milestone 9 — Healthcare Interoperability

Can explain external standard integration and internal domain mapping.

## Milestone 10 — AI Platform

Can integrate asynchronous AI inference with versioned results.

## Milestone 11 — Cloud Native

Can deploy and scale services through Kubernetes.

## Milestone 12 — Staff-Level Architecture Review

Can defend:

- architecture;
- scalability assumptions;
- service boundaries;
- consistency choices;
- failure handling;
- storage choices;
- performance trade-offs; and
- future evolution.

---

# 34. Portfolio / Interview Outcomes

By completion, the project should support discussion of the following.

## Product Story

> Designed and implemented a medical-imaging workflow platform covering order, scheduling, imaging ingestion, processing, AI analysis, radiologist worklist, and reporting.

## Architecture Story

> Started with a modular monolith and progressively introduced asynchronous processing, service decomposition, caching, search, observability, and cloud-native scaling based on measurable requirements.

## Distributed Systems Story

> Designed for duplicate delivery, retries, failure isolation, asynchronous processing, consistency trade-offs, and horizontally scalable workers.

## Healthcare Story

> Built practical understanding of MRI workflow, DICOM, imaging archives, radiology worklists, and interoperability concepts.

## Polyglot Engineering Story

> Used Java for core business services, C++ for imaging-related processing, Go for high-throughput infrastructure/worker components, and Python for AI inference.

## Staff-Level Story

Every major feature should ultimately answer:

```text
What problem were we solving?
What were the scale assumptions?
What were the alternatives?
Why was this design selected?
What are the trade-offs?
How does it fail?
How do we observe it?
How does it scale?
How would we evolve it later?
```

---

# 35. Project Working Model

## 35.1 Master Chat

Maintain one master project discussion for:

- PRD;
- roadmap;
- architecture;
- milestone status;
- cross-component decisions;
- phase tracking.

Suggested name:

**MedFlow — Master Architecture & Progress**

## 35.2 Component Chats

Create separate focused chats for LLD and implementation topics.

Examples:

```text
MedFlow — Scheduling LLD
MedFlow — DICOM Parser LLD
MedFlow — Report LLD
MedFlow — Kafka Architecture
MedFlow — Distributed Locking
MedFlow — AI Service
```

## 35.3 Input to Each Component Chat

For each component, provide:

- current MedFlow requirement;
- AlgoMaster link/topic;
- language-learning path link when relevant;
- current design/code.

The component workflow is:

```text
Relevant Revision
      |
      v
Concept Application
      |
      v
LLD
      |
      v
Implementation
      |
      v
Testing
      |
      v
System Design
      |
      v
Staff-Level Review
```

## 35.4 Bringing Decisions Back to the Master Chat

Once a component is completed, record its final:

- interfaces;
- service boundary;
- persistence ownership;
- event contracts;
- important trade-offs; and
- ADR reference.

This prevents architectural decisions from becoming isolated inside individual chats.

---

# 36. Future Enhancements

Potential future extensions after the core platform:

- multi-hospital tenancy;
- multi-site imaging centers;
- scanner fleet management;
- protocol/configuration management;
- study routing policies;
- advanced radiologist workload balancing;
- AI model marketplace-like architecture;
- research datasets;
- anonymization pipelines;
- image annotation workflows;
- analytics platform;
- operational reporting;
- data lake / warehouse integrations;
- disaster-recovery environment;
- multi-region deployment;
- edge processing near imaging devices;
- GPU scheduling;
- workflow orchestration engine.

These should remain backlog items until the core system is complete.

---

# 37. Glossary

| Term | Working Definition |
|---|---|
| **Patient** | Person associated with healthcare/imaging records in the educational system |
| **Imaging Order** | Request for an imaging examination |
| **Appointment** | Scheduled slot for an imaging examination |
| **Study** | Group representing an imaging examination in the MedFlow model |
| **Series** | Logical grouping within a study |
| **Instance** | Individual imaging object within a series |
| **DICOM** | Imaging and related information standard used in medical imaging |
| **MRI** | Magnetic resonance imaging modality |
| **PACS** | Picture Archiving and Communication System concept |
| **RIS** | Radiology Information System concept |
| **VNA** | Vendor-neutral imaging archive concept |
| **FHIR** | Healthcare interoperability standard/resource model |
| **HL7** | Family of healthcare interoperability standards |
| **DICOMweb** | Web-oriented interfaces for DICOM data |
| **Worklist** | Queue/list of studies/tasks to be reviewed or processed |
| **Object Storage** | Storage system optimized for large binary objects |
| **Idempotency** | Property that repeated execution produces the same intended result |
| **Eventual Consistency** | A model where replicas/projections converge rather than updating synchronously |
| **DLQ** | Dead-letter queue for messages that cannot be processed successfully |
| **SLO** | Service Level Objective used to define measurable reliability/performance targets |
| **ADR** | Architecture Decision Record |

---

# Appendix A — Initial Product Backlog

## MVP Backlog

```text
[ ] Repository setup
[ ] Product documentation
[ ] Domain glossary
[ ] Patient entity
[ ] Patient API
[ ] Imaging Order entity
[ ] Order API
[ ] Order state machine
[ ] Appointment entity
[ ] Scheduling API
[ ] Study entity
[ ] Study API
[ ] Report entity
[ ] Reporting API
[ ] PostgreSQL integration
[ ] Unit tests
[ ] Integration tests
[ ] Docker setup
```

## Imaging Backlog

```text
[ ] MRI simulator
[ ] Synthetic DICOM generator/input
[ ] DICOM parser
[ ] Metadata extraction
[ ] MinIO integration
[ ] DICOM ingestion service/module
[ ] Study/Series/Instance persistence
```

## Distributed Systems Backlog

```text
[ ] Kafka
[ ] Event schemas
[ ] Processing worker
[ ] Consumer groups
[ ] Retry policy
[ ] DLQ
[ ] Idempotency
[ ] Distributed locking
[ ] Redis
[ ] Search projection
[ ] Load balancing
[ ] Rate limiting
```

## Production Backlog

```text
[ ] Metrics
[ ] Tracing
[ ] Dashboards
[ ] RBAC
[ ] Audit service
[ ] CI/CD
[ ] Kubernetes
[ ] Autoscaling
[ ] Load tests
[ ] Failure tests
[ ] DR design
```

## Advanced Backlog

```text
[ ] FHIR integration
[ ] HL7 integration concept/demo
[ ] DICOMweb
[ ] Python AI service
[ ] Model versioning
[ ] GPU worker simulation
[ ] Multi-region design
[ ] Sharding experiment
[ ] Advanced workflow orchestration
```

---

# Appendix B — Architecture Learning Principle

The project should evolve according to the following sequence:

```text
1. Understand the domain
        ↓
2. Define requirements
        ↓
3. Model the domain
        ↓
4. Design the LLD
        ↓
5. Build a modular monolith
        ↓
6. Measure real bottlenecks
        ↓
7. Introduce asynchronous processing
        ↓
8. Split services where justified
        ↓
9. Add caching/search/scaling mechanisms
        ↓
10. Engineer for failure
        ↓
11. Add security and observability
        ↓
12. Add interoperability and AI
        ↓
13. Containerize and deploy on Kubernetes
        ↓
14. Load test and optimize
        ↓
15. Perform Staff-level architecture review
```

---

# Appendix C — Project Success Definition

MedFlow is considered a successful learning project when the implementation is not only functional, but when the engineer can explain the reasoning behind its evolution.

The strongest final outcome is the ability to demonstrate:

```text
Healthcare Domain Understanding
            +
Clean LLD
            +
Strong Backend Engineering
            +
Distributed Systems
            +
Scalability
            +
Reliability
            +
Observability
            +
Security
            +
AI Integration
            +
Cloud/Kubernetes
            +
Performance Engineering
            =
Staff-Level Engineering Project
```

---

**End of PRD v1.0**
