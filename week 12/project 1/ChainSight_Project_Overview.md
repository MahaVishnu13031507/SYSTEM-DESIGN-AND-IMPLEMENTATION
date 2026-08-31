Project 02 — ChainSight Smart IoT-Enabled Inventory & Supply Chain Management System

📌 Overview
A scalable, cloud-native IoT-integrated inventory and supply chain tracking platform designed to automate restocking and maintain real-time warehouse audits.

The system manages edge sensor data ingestion, event stream deduplication, stock tracking, automatic replenishment triggering, supplier procurement, and predictive forecasting.

🎯 Objective
To design a secure, reliable, and high-throughput IoT inventory management platform capable of ingesting high-frequency sensor streams (RFID, environmental) without database write locks or data duplication.

👥 Actors
Warehouse Staff
Procurement Manager
Supplier Partner
Edge RFID Scanner
Ingestion Service
Replenishment Service
Forecasting Service
Notification Service

🔑 Key Features
Student registration and authentication
Secure device authentication (mTLS)
Contactless RFID stock tracking
Low-latency edge-to-cloud stream ingestion
Tumbling-window event deduplication
Multi-warehouse inventory ledger
Safety threshold monitoring
Automated purchase order generation
Real-time monitoring dashboard
Telemetry event logging
Supplier catalog integration
Predictive demand forecasting

🏗️ Architecture
The system follows a service-oriented cloud architecture.

IoT Sensors / RFID Readers
   │
   ▼
Edge Gateway (MQTT)
   │
   ▼
Cloud IoT Ingestion Core
   │
   ├───────────────┬────────────────┐
   ▼               ▼                ▼
Auth Service    Ingestion Stream  Telemetry Stream
   │               │                │
   └───────────────┼────────────────┘
                   │
                   ▼
       Stream Deduplication (Kafka)
                   │
                   ▼
        Inventory Ledger Service
                   │
                   ▼
          Replenish Service
                   │
          ┌────────┴────────┐
          ▼                 ▼
    Supplier API        Analytics
          │
          ▼
       Database

☁️ Cloud Design
The system is designed to support:
Horizontal scaling
Load balancing
Distributed caching
Asynchronous processing
Database replication
Containerized services
Cloud-based monitoring
Fault tolerance

🔐 Security
Security mechanisms include:
Mutual TLS (mTLS) for IoT devices
HTTPS/TLS for web/mobile interfaces
JWT-based Role-Based Access Control (RBAC)
Customer Master Keys (KMS) envelope encryption
Secure device onboarding
API rate limiting
Immutable audit logging for ledger adjustments

🛠️ Proposed Technology Stack
Component	Technology
Frontend	React / Next.js
Backend	Spring Boot / Go
IoT Ingestion	AWS IoT Core / Azure IoT Hub
Database	PostgreSQL (Transactional) + Amazon Timestream (Telemetry)
Cache	Redis
Message Queue	Apache Kafka (MSK)
Authentication	JWT / mTLS (Devices)
Containers	Docker / Kubernetes (EKS)
Cloud	AWS / Azure
Monitoring	Prometheus + Grafana

📂 Project Files
project-02/
│
├── README.md
└── architecture.md

architecture.md
Contains the detailed High-Level Design including:
System architecture
Component architecture
Data flow
Service responsibilities
Database design
Scalability
Security
Fault tolerance
Cloud deployment
Technology stack

🎯 Expected Outcome
The final system should provide a high-throughput, cloud-deployed inventory management platform capable of processing real-time sensor updates, keeping transaction ledgers clean of duplicate noise, and automating restocking loops.

🔄 Development Flow
RFID Sensor Scan
      ↓
IoT Core Ingestion
      ↓
Stream Processing
      ↓
Deduplication Window
      ↓
Ledger Update
      ↓
Threshold Verification
      ↓
Automated Replenishment
      ↓
Purchase Order Generated
