# Literature Survey: Selected System Designs

## 1. RouteSync: Real-Time Geospatial Dispatch Optimization for Ride-Sharing

### Reference Paper Details
* **Title:** Optimizing Order Dispatch for Ride-Sharing Systems
* **Publication:** IEEE Conference Publication
* **Document Link:** [https://ieeexplore.ieee.org/abstract/document/8847177](https://ieeexplore.ieee.org/abstract/document/8847177)
* **Document ID:** 8847177

### Key Objectives & Methodology
The paper addresses inefficiencies in urban ride-sharing order dispatch systems. It proposes a dispatch framework that broadcasts passenger requests to drivers within an iteratively expanding dispatch region:
* **Hybrid Matching Model:** Combines system-assigning (platform-driven optimization) and driver-grabbing (driver autonomy) strategies to balance passenger wait times and driver earnings.
* **Iterative Expansion Algorithm:** Dynamically expands the geographical search boundary for an unmatched order, moving from localized geohash regions outward to neighboring areas.

### Identified Research Gaps & Limitations
* **Scale and Real-Time Infrastructure:** The paper focuses on the mathematical dispatch algorithm but does not address the underlying cloud architecture required to process thousands of continuous GPS coordinates per second.
* **Lack of Dynamic Surge Pricing:** The model does not integrate real-time surge pricing calculations linked to spatial-temporal demand shifts.

### Architectural Alignment (Relevance to RouteSync)
This paper provides the mathematical foundation for RouteSync's **Iterative Dispatch-Region Expansion Engine**. RouteSync implements this mathematical framework inside a cloud-native microservices architecture, leveraging AWS API Gateway WebSockets for real-time communication, Kafka/Kinesis for high-frequency location streaming, and Redis Geo/H3 geospatial grids for low-latency calculations.

---

## 2. ChainSight: Smart IoT Inventory and Predictive Supply Chain Platform

### Reference Paper Details
* **Title:** IoT Application for Smart Inventory Management System Based on RFID
* **Publication:** IEEE Conference Publication
* **Document Link:** [https://ieeexplore.ieee.org/document/10919846](https://ieeexplore.ie000e.org/document/10919846)
* **Document ID:** 10919846

### Key Objectives & Methodology
The paper focuses on solving accuracy and delay issues in warehouse inventories through automated, contactless scanning:
* **Contactless Tracking:** Uses Radio Frequency Identification (RFID) tags and scanners to track items as they move through warehouse entry and exit zones.
* **Edge-to-Centralized Transmission:** Details a configuration where edge scanners read RFID tags and transmit the raw IDs to a central database system for inventory updates.

### Identified Research Gaps & Limitations
* **Ingestion Scaling & Database Locks:** The paper assumes direct updates to a central database. In a large enterprise, high-frequency raw RFID scanner events cause database write locks and network bottlenecks.
* **Lack of Event Deduplication:** There is no mechanism described for filtering noisy, redundant RFID scans at the edge or ingestion layer.

### Architectural Alignment (Relevance to ChainSight)
This paper provides the sensor and edge scanner framework for ChainSight. ChainSight addresses the scalability gaps by introducing a **High-Throughput IoT Event-Deduplication and Ledger Update Pipeline**. Using AWS IoT Core with mTLS, event stream processing (Kafka/Kinesis), and specialized databases (Amazon Timestream for telemetry, PostgreSQL for transactional inventory), ChainSight solves the database lock and stream noise problems left unaddressed in the paper.

---

## Comparative Literature Survey Matrix 

| Aspect | Case Study 1: RouteSync (Ride-Sharing) | Case Study 2: ChainSight (IoT Inventory) |
| :--- | :--- | :--- |
| **Base IEEE Paper** | "Optimizing Order Dispatch for Ride-Sharing Systems" | "IoT Application for Smart Inventory Management System Based on RFID" |
| **Primary Method** | Iterative region expansion & hybrid dispatch allocation. | RFID-based contactless inventory scans at entry/exit gates. |
| **Technical Limitation** | Lacks low-latency cloud stream routing architecture. | High-frequency scan noise and write bottlenecks on central databases. |
| **Proposed Innovation** | Real-time WebSockets with local spatial-temporal H3 grids. | Window-based stream deduplication and Polyglot database persistence. |
| **Cloud Target** | AWS API Gateway, EKS (Kubernetes), Redis Geo, Kinesis. | AWS IoT Core, Managed Kafka (MSK), Amazon Timestream. |
