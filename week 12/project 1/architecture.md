Project 02 — High-Level Architecture
ChainSight Smart IoT-Enabled Inventory & Supply Chain Management System

1. System Overview
The ChainSight Smart IoT-Enabled Inventory & Supply Chain Management System is a cloud-native platform for real-time stock tracking and automated supply chain replenishment.

The architecture separates core logistics and high-frequency sensor ingestion functions into independent, highly scalable microservices, ensuring that heavy telemetry processes do not impact day-to-day transactional database operations.

2. Architecture Goals
The architecture is designed to provide:
Scalability
High Event-Throughput (IoT Ingestion)
Stream Deduplication & Data Cleanliness
High Availability & Fault Tolerance
Zero-Trust Security (mTLS device authentication)
Low-Latency Inventory Reads
Asynchronous Restocking & Procurement Execution
Maintainability

3. High-Level Architecture
                         ┌─────────────────────┐
                         │   RFID / SENSORS    │
                         │    Edge Devices     │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Edge Gateway     │
                         │    MQTT Protocol    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Cloud IoT Core   │
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
     ┌────────────────┐   ┌────────────────┐   ┌────────────────┐
     │ Authentication │   │ Ingestion      │   │ Inventory      │
     │    Service     │   │ Service        │   │ Service        │
     └───────┬────────┘   └───────┬────────┘   └───────┬────────┘
             │                    │                    │
             └────────────────────┼────────────────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │  Stream Deduplicator│
                       └──────────┬──────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │    Message Queue    │
                       │    Apache Kafka     │
                       └──────────┬──────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │ Replenish Service   │
                       └──────────┬──────────┘
                                  │
                         ┌────────┴────────┐
                         ▼                 ▼
                ┌────────────────┐ ┌────────────────┐
                │ Supplier API   │ │   Analytics    │
                │    Service     │ │    Service     │
                └───────┬────────┘ └────────────────┘
                        │
                        ▼
               ┌────────────────────┐
               │     PostgreSQL     │
               │      Database      │
               └─────────┬──────────┘
                         │
                         ▼
                  ┌──────────────┐
                  │ Redis Cache  │
                  └──────────────┘

4. Component Responsibilities

4.1 Edge Gateway
Aggregates raw tag scans, translates RF protocols to lightweight MQTT payloads, and buffers messages locally during network outages.

Responsibilities
Protocol translation
Signal aggregation
Local data buffering
Edge security enforcement

4.2 Cloud IoT Core
Secure entry point for edge messages. Authenticates edge devices via mTLS, enforces connection policies, and routes streams to downstream ingestion queues.

Responsibilities
Device provisioning
Mutual TLS (mTLS) authentication
MQTT topic routing
Message rule processing

4.3 Authentication Service
Manages system identity and access management for warehouse managers, suppliers, and administration staff.

Responsibilities
Login / MFA
Token generation (JWT)
Role management
Session validation

Roles
Warehouse Staff
Procurement Manager
Supplier Partner
Administrator

4.4 Ingestion Service
Receives raw telemetry and scan events. Coordinates buffer storage in Kafka queues before deduplication logic is applied.

Responsibilities
Receive HTTP/MQTT payloads
Validate payloads
Format event schema
Queue messages

4.5 Inventory Service
Manages SKU configurations, catalogs, and localized warehouse metrics.

Responsibilities
SKU profile management
Stock allocation
Warehouse tracking
Inventory catalog administration

4.6 Stream Deduplicator
Groups incoming scans within sliding windows to remove duplicate tag reads before they hit the core transactional database.

Responsibilities
Sliding-window buffering
RFID tag deduplication
Noise filtering
Deduplicated event output

4.7 Message Queue
Uses Apache Kafka to decouple high-speed IoT data producers from slow database consumers.

Responsibilities
Decouple services
Buffer telemetry streams
Ensure message ordering
Provide reliable event processing

Example:
Sensor Scan
       ↓
Ingestion Service
       ↓
Stream Deduplicator
       ↓
Inventory Ledger

4.8 Replenish Service
Monitors real-time stock balances and automatically initiates a procurement workflow when stock levels dip.

Responsibilities
Threshold monitoring
Purchase order drafting
Restocking event tracking
Supplier selection routing

4.9 Supplier Service
Communicates with external supplier systems to generate Purchase Orders (PO) and tracks delivery statuses.

Responsibilities
API integration
Order transmission
Supplier status monitoring
Delivery scheduling updates

4.10 Analytics Service
Stores historic data trends in time-series formats to generate demand forecasting, shrinkage logs, and pipeline latency metrics.

Responsibilities
Time-series logging
Forecast modeling
Shrinkage alert triggering
System performance tracking

5. Data Flow
1. RFID Sensor Scan
       ↓
2. Edge Gateway (MQTT)
       ↓
3. Cloud IoT Core (mTLS)
       ↓
4. Ingestion Service
       ↓
5. Stream Deduplicator (Kafka)
       ↓
6. Inventory Ledger Service
       ↓
7. Replenish Service
       ↓
8. Message Queue
       ↓
9. Supplier Service
       ↓
10. Database (PostgreSQL / Time-Series)
       ↓
11. Updated Stock Returned to Dashboard

6. Database Design
Main Entities

User
----
user_id
name
email
password_hash
role
created_at

SKU / Item
----------
sku
name
description
category
safety_threshold
reorder_quantity

Warehouse_Stock
---------------
warehouse_id
sku
current_quantity
reserved_quantity
status

Sensor_Reading
--------------
reading_id
reader_id
sku
temperature
weight
recorded_at

Purchase_Order
--------------
po_id
supplier_id
sku
quantity
status
ordered_at
expected_delivery_at

7. Scalability Strategy
The system must support high-frequency sensor readings across multiple distributed warehouse zones.

Horizontal Scaling
Ingestion, ledger, and forecasting pods are containerized and scaled dynamically based on event-stream lag and CPU usage.

              Load Balancer
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     Server 1    Server 2    Server 3

Caching
Redis caches hot warehouse stock levels, active SKU lookups, and session tokens to offload PostgreSQL reads.

Asynchronous Processing
Stream deduplication and supplier purchase order generation are fully asynchronous, utilizing Kafka streams to decouple processing layers.

Database Scaling
High-volume telemetry uses partitioned time-series storage (Amazon Timestream), while relational data uses read replicas and connection pooling.

8. Security Architecture
Edge Devices / Dashboard
  ↓
mTLS / HTTPS
  ↓
Cloud IoT Core / Gateway
  ↓
Authentication (JWT)
  ↓
Authorization (RBAC)
  ↓
Application Microservices
  ↓
Encrypted Databases (KMS)

Security Controls
Mutual TLS (mTLS) for IoT devices
HTTPS/TLS for client portals
JWT-based Role-Based Access Control (RBAC)
Customer Master Keys (KMS) envelope encryption
Secure device onboarding
API rate limiting
Immutable audit logging for ledger adjustments

9. Inventory Integrity
Potential mechanisms include:
Tumbling-window deduplication to filter redundant scans
Environmental logs (temp/humidity) tracked via time-series
Local edge cache verification to prevent data drift
Secure audit logs for all manual ledger overrides
RFID signal strength (RSSI) validation to confirm item presence

10. Fault Tolerance
The system should continue operating even if an individual component fails.

Edge Offline
      │
      ▼
Local Edge Buffer
      │
      ▼
Network Reconnection
      │
      ▼
Cloud Ingestion Queue (Kafka)
      │
      ▼
Ledger Sync Complete

The Kafka message queue also provides buffering when downstream ledger services temporarily become unavailable, ensuring zero telemetry events are lost.

11. Cloud Deployment
                         Edge Scanners
                               │
                               ▼
                        Cloud IoT Core
                               │
                               ▼
                          API Gateway
                               │
                 ┌─────────────┼─────────────┐
                 ▼             ▼             ▼
             Auth Pods    Ingest Pods   Ledger Pods
                 │             │             │
                 └─────────────┼─────────────┘
                               ▼
                         Message Queue (Kafka)
                               │
                               ▼
                         Replenish Pods
                               │
                               ▼
                          PostgreSQL
                               │
                               ▼
                       Amazon Timestream

12. Technology Stack
Layer	Technology
Client	React / Next.js
Backend	Spring Boot / Go
API Gateway	AWS API Gateway
Load Balancer	Cloud Load Balancer
Database	PostgreSQL (Transactional) + Amazon Timestream (Telemetry)
Cache	Redis
Message Queue	Apache Kafka (MSK)
Authentication	JWT (Users) + mTLS certificates (IoT)
Container	Docker
Orchestration	Kubernetes (EKS)
Cloud	AWS / Azure / GCP
Monitoring	Prometheus + Grafana

13. Non-Functional Requirements
Performance
The system should provide low-latency reads for dashboard metrics and support high-frequency sensor writes.

Scalability
The system should support horizontal scaling during peak delivery seasons.

Availability
Core inventory services must be highly available to prevent processing blocks.

Security
Sensitive supplier credentials and device telemetry data must be protected.

Reliability
IoT sensor scan events must not be lost because of temporary system outages.

Maintainability
Logistics components should be independently deployable.

14. Future Enhancements
Possible future extensions include:
Blockchain-based supply chain trace verification
Computer-vision analysis for stock status estimation
Automated shelf-picking robot path optimization
ML-driven automated dynamic supplier allocation
Cross-docking route scheduling
Multi-region data synchronization

15. Final Architecture Summary
                    CHAINSIGHT INVENTORY SYSTEM

                       IoT Sensors / RFID
                              │
                              ▼
                         Edge Gateway
                              │
                              ▼
                        Cloud IoT Core
                              │
                              ▼
                         API Gateway
                              │
            ┌─────────────────┼─────────────────┐
            ▼                 ▼                 ▼
         Auth             Ingestion           Ledger
        Service            Service           Service
            │                 │                 │
            └─────────────────┼─────────────────┘
                              ▼
                     Stream Deduplicator
                              │
                              ▼
                        Message Queue
                              │
                              ▼
                      Replenish Service
                              │
                     ┌────────┴────────┐
                     ▼                 ▼
               Supplier Service    Analytics
                     │              Service
                     ▼
                 PostgreSQL
                     │
                     ▼
                  Redis Cache

16. Conclusion
The proposed architecture provides a scalable, secure, and reliable foundation for a real-time IoT inventory and supply chain tracking platform.

By decoupling telemetry data stream processes, removing duplicate reads, and triggering reorders asynchronously, the platform guarantees ledger consistency and reduces operational restocking overhead.
