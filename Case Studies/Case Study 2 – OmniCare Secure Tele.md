Case Study 2 – OmniCare Secure Telehealth and Compliance-First Consultation Platform
1. Introduction
Patients in rural and underserved areas face significant challenges accessing medical specialists, leading to delayed treatment and high travel costs. Although virtual consultations offer a viable alternative, designing telehealth platforms that scale dynamically while guaranteeing compliance with strict healthcare data privacy regulations remains a complex challenge.

2. Core Problem
Medical systems handle Protected Health Information (PHI) subject to rigid regulations such as HIPAA and GDPR. Fragmented telemedicine solutions often struggle to maintain low-latency, high-quality audio/video communication while ensuring rigorous audit logging, field-level database encryption, and strict role-based isolation of sensitive records.

The problem to investigate is:

How can a cloud system deliver secure, low-latency WebRTC video consultations while maintaining strict regulatory compliance and structured electronic health record integrity at scale?

3. Proposed Solution
OmniCare is proposed as a HIPAA-aware, cloud-native telemedicine platform. Instead of using standard, unencrypted video conferencing tools, it orchestrates and secures clinical workflows using:

Secure WebRTC/SFU video consultation streaming
Role-based access control with multi-factor authentication (MFA)
HL7 FHIR-aligned Electronic Health Record (EHR) models
AI-driven symptom triage and specialist routing
Envelope encryption with Customer Master Keys (KMS)
The system enforces compliance-aligned access controls.

4. Proposed Technical Innovation
The project will investigate a Compliance-Isolated Real-Time Consultation Architecture. The important research direction is to determine how to decouple low-latency media routing from clinical workflow services while ensuring all PHI access events are immutably logged and encrypted.

Example:

Patient login (MFA) → AI symptom intake → secure WebRTC video channel generation → consultation with doctor → electronic prescription generation → HL7 FHIR record update → KMS-backed audit log entry.

5. Cloud Deployment
The platform can be deployed using a cloud API gateway, WebRTC/SFU media servers (AWS Chime SDK / Azure Communication Services), containerized EHR services on managed Kubernetes, relational database (PostgreSQL) with field-level encryption, NoSQL document store (MongoDB Atlas), KMS key management service, and secure blob storage with pre-signed URLs.

6. Expected Impact
OmniCare could help healthcare organizations expand remote patient access, guarantee complete HIPAA/GDPR regulatory compliance, secure clinical logs against unauthorized access, and streamline physician workflows.

7. Case Study Takeaway
The project investigates a shift from “fragmented, uncoordinated virtual visits” to “compliance-first, end-to-end encrypted clinical consultation workflows.”