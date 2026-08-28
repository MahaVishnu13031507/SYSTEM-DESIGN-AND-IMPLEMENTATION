Case Study 4 – NetSentry Cloud-Based Firewall Rule Analyzer and Security Policy Verification Engine

1. Introduction

Modern enterprise networks run on multi-vendor, multi-cloud firewalls containing thousands of access control rules. As applications scale and evolve, these firewall rulebases become excessively complex, leading to shadowing, redundancy, and hidden security gaps that are difficult to identify manually.

2. Core Problem

Security operations teams lack unified, real-time visibility into active firewall policies. Static configurations often contain conflicting rules, open ports, or deprecated access permissions, exposing cloud infrastructure to attacks and violating compliance requirements.

The problem to investigate is:

How can a cloud system automatically parse, normalize, and analyze multi-vendor firewall configuration rules to identify policy conflicts and generate audit-ready security metrics in real time?

3. Proposed Solution

NetSentry is proposed as a cloud-based firewall rule analysis and policy verification platform. Instead of manual configuration reviews, it parses and analyzes security policies using:

* Multi-format configuration parsers (Cisco, Fortinet, Palo Alto, AWS Security Groups)
* Rule shadowing and anomaly detection algorithms
* Multi-dimensional policy visualization dashboards
* Automated compliance auditing against security standards (ISO 27001, PCI-DSS)
* PDF-based security posture reporting

The system produces policy audits and compliance reports.

4. Proposed Technical Innovation

The project will investigate an Acyclic Graph-Based Firewall Rule Shadowing Detection Model. The important research direction is to determine how to represent firewall rules as directed graphs or interval trees to mathematically identify redundant, shadowed, or conflicting rules.

Example:

Raw config upload → normalization to standard JSON schema → interval tree construction → intersection check (shadowing/redundancy) → policy violation alert → PDF report generation.

5. Cloud Deployment

The platform can be deployed using a cloud API gateway, Flask backend containerized on managed Kubernetes, Celery worker nodes for asynchronous parsing, Redis for task queueing, PostgreSQL for user and policy metadata, and S3/Blob Storage for raw configuration files and generated PDF reports.

6. Expected Impact

NetSentry could help organizations identify hidden security vulnerabilities, reduce firewall policy bloat, ensure continuous audit compliance, and automate security reporting for network administrators.

7. Case Study Takeaway

The project investigates a shift from “periodic manual firewall reviews” to “automated, graph-based security policy anomaly detection.”