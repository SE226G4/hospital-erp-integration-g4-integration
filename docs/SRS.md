# Software Requirements Specification (SRS)

## [cite_start]Project: Integrated Hospital Resource Management System (Hospital ERP) [cite: 2]
## Module/Subsystem: Master Governance & Integration Document (All 7 Modules)
Version: 1.0  
Date: 2026-05-23  

---

## 1. Introduction

### 1.1 Purpose
[cite_start]The purpose of this document is to define the complete functional and non-functional requirements for the comprehensive Hospital ERP system[cite: 2]. [cite_start]This acts as a centralized master governance document binding the Integration Team (Team Leaders) and all 7 sub-development teams [cite: 4, 14, 24, 31, 38, 47, 53] to ensure unified architectural standards, data integrity, and cross-departmental constraints.

### 1.2 Scope
[cite_start]The Hospital ERP system integrates clinical, financial, and logistical workflows into a synchronized digital ecosystem[cite: 6, 16, 26, 31, 40, 49, 53].
* What the System WILL Do (In-Scope):
    * [cite_start]Establish a single, unified patient record using the National ID as the unique key to prevent duplicate profiles[cite: 8].
    * [cite_start]Enforce strict cross-module dependencies (e.g., verifying allergies before dispensing medication [cite: 34][cite_start], checking financial coverage before providing services [cite: 20]).
    * [cite_start]Coordinate live bed tracking, automated insurance coverage matching, surgical scheduling optimization, and real-time inventory deductions[cite: 19, 27, 33, 42].
* What the System Will NOT Do (Out-of-Scope):
    * The system will not directly operate or control biomedical hardware devices (such as operating room machinery or laboratory analyzers), but it will accept data logs and electronic reports transmitted from them.

### 1.3 Definitions, Acronyms, and Abbreviations

| Acronym | Module/Term Name | Definition |
| :--- | :--- | :--- |
| ERP | Enterprise Resource Planning | [cite_start]The overarching system managing the hospital's resources, finances, and medical data[cite: 2]. |
| ADM-MC | Admission & Medical Coding | [cite_start]Module 1: Handles unique patient registration, digital health identities, and risk profile logs[cite: 5, 6, 9]. |
| FIN-INS | Finance & Insurance | [cite_start]Module 2: Consolidates service costs into a unified bill and automates insurance clearance[cite: 15, 16, 17, 19]. |
| IPD-BED | Inpatient & Bed Management | [cite_start]Module 3: Tracks real-time ward bed status (occupied, available, cleaning) and patient distribution[cite: 25, 26, 27]. |
| PHM-LOG | Pharmacy Logistics & MAR | [cite_start]Module 4: Manages medication dispensing, automated safety checks, and stock deductions[cite: 31, 33, 34]. |
| SURG-OPT | Surgical Optimization | [cite_start]Module 5: Governs operating room scheduling, resource allocation, and sterile downtime[cite: 39, 40, 42, 44]. |
| ER-FLOW | Emergency Flow & Triage | [cite_start]Module 6: Categorizes emergency cases based on medical urgency and clinical priorities[cite: 48, 49, 50, 51]. |
| INV-SUP | Inventory & Supplies Management | [cite_start]Module 7: Tracks core medical stock levels, electronic supply requests, and item expirations[cite: 53, 55, 57, 59]. |

### 1.4 References
* IEEE 830 Standard for Software Requirements Specifications.
* Shared Architectural Design Documents and central API Contracts agreed upon by the Integration Team and Module Leads.

### 1.5 Overview
This document is structured into four main parts: Section 2 outlines the global system perspective, shared interfaces, and overall product dependencies. [cite_start]Section 3 details specific functional requirements mapped as Agile User Stories for each of the 7 modules[cite: 4, 14, 24, 31, 38, 47, 53]. Section 4 includes tracking appendices and system checklists.

---

## 2. Overall Description
### 2.1 Product Perspective
The Hospital ERP is a modular architecture wherein all 7 subsystems communicate securely over a centralized API Gateway. [cite_start]No sub-module operates in complete isolation; they rely on the database constraints of the master system to enforce patient safety and financial validity[cite: 11, 21, 30, 34, 36, 43, 44, 51, 58, 59].

* [cite_start]2.1.1 System Interfaces: Inter-module communication (e.g., Pharmacy pushing costs to Finance [cite: 36][cite_start], Wards querying Room Availability [cite: 43]) is managed via a shared, authenticated API Gateway.
* 2.1.2 User Interfaces: All modules must conform to a single, shared UI Design System (consistent color-coding, typography, and layout rules) to ensure a seamless experience for medical staff navigating between subsystems.
* [cite_start]2.1.3 Hardware Interfaces: Integration with barcode scanners (for scanning medication IDs and tracking inventory)[cite: 33, 56], and patient wristband printers at the admission desk.
* 2.1.4 Software Interfaces: Centralized Relational Database System (e.g., PostgreSQL or MySQL) running on a secure cloud/on-premise server environment.
* [cite_start]2.1.5 Communications Interfaces: Secure HTTP/REST protocols for data exchanges, alongside WebSockets for real-time critical alerts (such as incoming red-alert emergency notifications)[cite: 51, 52, 58].
* [cite_start]2.1.6 Memory & Operational Constraints: Minimum 99.99% system uptime requirement given the life-critical operations within Emergency and Surgical suites[cite: 42, 50].

### 2.2 Product Functions
* [cite_start]Unified registration preventing multi-profile entry using the National ID[cite: 8].
* [cite_start]Automatic aggregation of multi-departmental services (inpatient, surgeries, pharmacy) into a single invoice[cite: 17, 18].
* [cite_start]Dynamic visual tracking of hospital bed states: Occupied, Available, and Under-Cleaning[cite: 27].
* [cite_start]Automated, allergy-aware prescription cross-referencing prior to pharmaceutical dispensing[cite: 34].
* [cite_start]Conflict-free surgical slot booking factoring in medical staff, tools, beds, and mandatory sterilization buffers[cite: 42, 43, 44].
* [cite_start]Urgency-based emergency sorting (Critical, Moderate, Mild) integrated with physical bed allocation[cite: 51].
* [cite_start]Electronic inventory requisitioning with dynamic minimum-threshold alarms and expiration tracking[cite: 57, 58, 59].

### 2.3 User Characteristics
[cite_start]Users vary from medical admission clerks (average technical skills) [cite: 8] [cite_start]to specialized clinical staff, surgeons, and nurses (high speed and clinical accuracy needs) [cite: 43, 51][cite_start], pharmacists and warehouse managers (inventory focus) [cite: 33, 58][cite_start], and finance officers (high financial accuracy requirements)[cite: 19].

### 2.4 Constraints, Assumptions, and Dependencies
* [cite_start]Core Dependency: All system workflows depend entirely on Module 1 (ADM-MC) successfully creating the single medical file[cite: 11]. [cite_start]No service can be executed, nor can any department accept a patient, without validating their digital health identity first[cite: 11].
* Regulatory Compliance: Strict compliance with healthcare data protection acts and patient privacy regulations.

---

## 3. Specific Requirements (Agile Approach)

### 3.1 External Interface Requirements
Data structures passed between modules must be strictly validated. For instance, when transferring an emergency patient to an inpatient room or scheduling a lab order, the payloads are transmitted as JSON objects containing standardized fields: National ID, Patient Identity Token, and Risk Flags.

### 3.2 System Features & User Stories
#### [cite_start]3.2.1 Module 1: Admission & Medical Coding (ADM-MC) [cite: 5, 6]
* [cite_start]Description: Governs unique patient profile generation and sets up mandatory baseline medical risk profiles[cite: 8, 9].
* Priority: Critical.
* User Stories:
    * [cite_start]Story 1: As an *Admission Clerk*, I want to *register a patient using their National ID*, so that *the system blocks creation if a record already exists, preventing duplicate patient logs*[cite: 8].
        * [cite_start]*Acceptance Criteria:* The system queries the centralized database; if the National ID is found, it denies a new entry and retrieves the existing record[cite: 8].
        * *GitHub Issue:* #ADM-01
    * [cite_start]Story 2: As a *Receiving Nurse/Doctor*, I want to *fully fill out the patient's risk profile (Allergies, Blood Type, Chronic Illnesses)*, so that *the patient can be safely cleared for admission into Wards, Surgery, or Emergency*[cite: 9, 10].
        * [cite_start]*Acceptance Criteria:* The system generates a digital medical identity; access to specific department workflows is blocked if the risk profile fields are left empty[cite: 9].
        * *GitHub Issue:* #ADM-02

#### [cite_start]3.2.2 Module 2: Finance & Insurance (FIN-INS) [cite: 15, 16]
* [cite_start]Description: Aggregates costs from all active modules and calculates insurance ratios[cite: 17, 19].
* Priority: High.
* User Stories:
    * [cite_start]Story 1: As a *Billing Officer*, I want *the system to automatically capture and append charges from multiple departments (Accommodation, Surgery, Pharmacy) into one single invoice*, so that *patient billing is unified and error-free*[cite: 17, 18].
        * [cite_start]*Acceptance Criteria:* Receives real-time transactional logs from modules IPD-BED, SURG-OPT, and PHM-LOG and appends them instantly to the patient's master account[cite: 18, 36].
        * *GitHub Issue:* #FIN-01
    * [cite_start]Story 2: As an *Insurance Clerk*, I want *the system to verify insurance eligibility and coverage ratios automatically*, so that *uncovered paid services are blocked unless direct payment is processed*[cite: 19, 20].
        * [cite_start]*Acceptance Criteria:* The system restricts any paid service deployment until a positive insurance confirmation or a direct payment log is updated; blocks final invoice generation if any service from another sub-system remains unrecorded[cite: 20, 21].
        * *GitHub Issue:* #FIN-02

#### [cite_start]3.2.3 Module 3: Inpatient & Bed Management (IPD-BED) [cite: 25, 26]
* [cite_start]Description: Monitors live hospital room capacities and manages synchronized admission/discharge notifications[cite: 27, 30].
* Priority: High.
* User Stories:
    * [cite_start]Story 1: As a *Ward Nurse*, I want to *view the real-time status of each bed (Occupied, Available, Under-Cleaning)*, so that *patients are only assigned or transferred to a room that is fully ready*[cite: 27, 28, 29].
        * [cite_start]*Acceptance Criteria:* Blocks manual/automated transfers if the destination room status is not explicitly set to "Available/Ready"[cite: 28, 29].
        * *GitHub Issue:* #BED-01
    * [cite_start]Story 2: As an *Inpatient Coordinator*, I want *the system to send a pre-discharge notice to Finance 24 hours prior to the expected checkout time*, so that *the final bill can be generated using valid data from the admission system*[cite: 30].
        * [cite_start]*Acceptance Criteria:* Triggers an automated alert payload to module FIN-INS exactly 24 hours before the recorded discharge estimation window[cite: 30].
        * *GitHub Issue:* #BED-02
#### [cite_start]3.2.4 Module 4: Pharmacy Logistics & MAR (PHM-LOG) [cite: 31]
* [cite_start]Description: Handles safe clinical drug dispensing, safety cross-checking, and real-time inventory linkage[cite: 33, 34].
* Priority: Critical.
* User Stories:
    * [cite_start]Story 1: As a *Pharmacist*, I want *the system to validate the prescription against the patient’s logged medical profile and active allergies*, so that *harmful cross-reactions are automatically blocked*[cite: 34].
        * [cite_start]*Acceptance Criteria:* Evaluates chemical components against the "Allergies" data from Module 1 (ADM-MC); blocks dispensing action with an alert if a match occurs[cite: 34].
        * *GitHub Issue:* #PHM-01
    * [cite_start]Story 2: As a *Pharmacy Dispenser*, I want *the system to automatically deduct stock levels and push financial data to billing during dispensing*, so that *inventory updates instantly and billing is tracked*[cite: 33, 35, 36].
        * [cite_start]*Acceptance Criteria:* System blocks dispensing if inventory is insufficient [cite: 35][cite_start]; updates stock count instantly and sends billing records to FIN-INS upon confirmation[cite: 33, 36].
        * *GitHub Issue:* #PHM-02

#### [cite_start]3.2.5 Module 5: Surgical Optimization (SURG-OPT) [cite: 39, 40]
* [cite_start]Description: Manages conflict-free operating theatre coordination, staff availability, and cleanup downtimes[cite: 42, 44].
* Priority: High.
* User Stories:
    * [cite_start]Story 1: As a *Surgical Coordinator*, I want to *schedule an operation only after validating medical team availability, tool readiness, post-op bed booking, and sterilization buffers*, so that *there are zero booking schedule overlaps*[cite: 42, 43, 44].
        * [cite_start]*Acceptance Criteria:* Cross-checks availability indices across IPD-BED (for post-op beds) [cite: 43] [cite_start]and enforces an unskippable "Mandatory Sterile Cleanup Time" block between back-to-back surgeries in the same room[cite: 44].
        * *GitHub Issue:* #SURG-01

#### [cite_start]3.2.6 Module 6: Emergency Flow & Triage (ER-FLOW) [cite: 48, 49]
* [cite_start]Description: Filters arriving emergency cases by physical severity instead of arrival time stamps[cite: 50, 51].
* Priority: Critical.
* User Stories:
    * [cite_start]Story 1: As a *Triage Nurse*, I want to *classify patients by risk severity (Critical, Moderate, Mild) so that critical cases are instantly matched to available beds*, so that *life-saving speed is prioritized*[cite: 51].
        * [cite_start]*Acceptance Criteria:* Setting a case to "Critical" queries IPD-BED instantly for an open emergency bed [cite: 51][cite_start]; if none are available, the system overrides default flows to push high-priority alarms to clinical coordinators to adjust internal priorities[cite: 51, 52].
        * *GitHub Issue:* #ER-01
#### [cite_start]3.2.7 Module 7: Inventory & Supplies Management (INV-SUP) [cite: 53]
* [cite_start]Description: Oversees central supply lines, processes departmental restock orders, and controls material expiration[cite: 55, 57, 59].
* Priority: Medium to High.
* User Stories:
    * [cite_start]Story 1: As an *Inventory Controller*, I want to *receive electronic supply requests from the Pharmacy and Surgical wings*, so that *items are securely verified and subtracted from the main stock upon delivery*[cite: 56, 57, 58].
        * [cite_start]*Acceptance Criteria:* Consumes fulfillment order requests from PHM-LOG and SURG-OPT [cite: 56, 57][cite_start]; decreases counts seamlessly on confirmation of physical release[cite: 58].
        * *GitHub Issue:* #INV-01
    * [cite_start]Story 2: As a *Warehouse Auditor*, I want *automated warnings for low stock levels and near-expiry items along with simple financial reports*, so that *re-orders are handled timely and zero expired goods are deployed*[cite: 58, 59].
        * [cite_start]*Acceptance Criteria:* Generates automated alerts when item counts hit minimum safety thresholds or cross safety dates [cite: 58, 59][cite_start]; dispatches automated consumption summaries to FIN-INS[cite: 59].
        * *GitHub Issue:* #INV-02

---

### 3.3 Performance Requirements
* [cite_start]Cross-Module API Latency: Inter-module queries (e.g., Pharmacy validating a patient profile via ADM-MC [cite: 34]) must process and respond within under 200ms.
* [cite_start]Real-time Synchronization: Stock deductions and transactional pricing postings must follow synchronous transactions to prevent data latency anomalies across billing or inventory systems[cite: 18, 33].

### 3.4 Logical Database Requirements
The centralized database splits operations across distinct tables mapped to specific module ownership boundaries:
* [cite_start]Module 1 Owned: patients, medical_risk_factors[cite: 8, 9].
* [cite_start]Module 2 Owned: invoices, insurance_agreements, payments[cite: 17, 19].
* [cite_start]Logistics & Medical Tables: hospital_beds, pharmacy_inventory, surgical_schedules, central_stock[cite: 27, 33, 42, 56].

### 3.5 Software System Attributes
* Security: Enforced multi-layered Role-Based Access Control (RBAC). [cite_start]Patient allergy and risk fields are shared globally for clinical safety[cite: 9, 34, 44, 51], but granular billing logs or operational schedules remain restricted to certified staff. All National IDs and health data parameters are encrypted at rest and in transit.
* System Fault-Tolerance: Code architectures across the 7 teams must employ independent error catching mechanisms, ensuring an unhandled issue inside an isolated module cannot crash the centralized system gateway.

---

## 4. Appendices

### Appendix A: Glossary & Models
* *This section embeds the centralized Entity-Relationship Diagram (ERD) mapping relational keys across the 7 modules, along with Data Flow Diagrams (DFDs) tracing the patient's clinical path.*

### Appendix B: GitHub Traceability Checklist
Before final sign-off, each team lead must verify the following:
* [ ] The module repository is successfully initialized under the primary project organization.
* [cite_start][ ] Every single User Story listed in Section 3.2 maps directly to an active GitHub Issue with proper labels[cite: 8, 9, 18, 20, 27, 30, 33, 34, 42, 51, 57, 59].
* [ ] Inter-module API endpoints have passed cross-verification tests and strictly respect the core safety block rules defined by the client.
