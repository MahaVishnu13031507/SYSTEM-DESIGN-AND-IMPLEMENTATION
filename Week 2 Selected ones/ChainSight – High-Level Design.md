ChainSight – High-Level Design
Purpose
ChainSight is a cloud-native, IoT-integrated inventory and supply chain tracking platform designed to automate restocking and maintain real-time warehouse audits.

Core Problem
Traditional inventory management relies on manual scans or batch updates, leading to stock inaccuracies, delayed reordering, and zero visibility across distributed warehouses. Legacy databases struggle to ingest continuous RFID/sensor events at scale, resulting in write bottlenecks and missed stockouts.

Proposed Architecture
IoT Sensors / RFID Readers ↓ Edge Gateway (MQTT) ↓ Cloud IoT Ingestion Core ↓ Event Stream Deduplication Service ↓ Real-Time Inventory Ledger Service ↓ Threshold Restocking Engine ↓ Supply Chain Integration Service ↓ Time-Series Telemetry Store ↓ Relational SKU Database ↓ Push Notification Queue ↓ Operations Dashboard

Major Components
Edge Protocol Translation Gateway
Cloud IoT Core Ingestion Point
Stream Processing Event Deduplicator
Real-Time Inventory Ledger Service
Automated Replenishment Engine
Supplier Catalog & Order Manager
Predictive Demand Forecasting Service
Relational Database (Transactional)
Time-Series Telemetry Store
Message Queue & Dispatcher
Warehouse Operations Dashboard
Proposed Innovation Direction
The system will investigate a High-Throughput IoT Event-Deduplication and Ledger Update Pipeline. The important research direction is to determine how to run low-latency event buffering and window-based deduplication of high-frequency RFID/sensor pings at the ingestion layer, preventing database locks and maintaining a reliable, real-time multi-warehouse inventory ledger.

The system will evaluate historical stock depletion rates and sensor trends to automatically calculate dynamic reorder thresholds and forecast supplier lead times.

Cloud Deployment
The architecture is designed as a cloud-native service using secure IoT device gateways with mutual TLS (mTLS), managed event streaming, containerized ledger microservices, high-speed time-series databases, in-memory caching, and serverless notification functions.

Expected Outcome
A prototype capable of processing simulated IoT inventory events, updating stock ledgers in real time, and auto-generating supplier purchase orders when inventory thresholds are crossed.