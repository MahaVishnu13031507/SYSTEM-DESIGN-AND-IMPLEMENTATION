# Literature Survey: Selected System Designs


## 1. RouteSync: Real-Time Geospatial Dispatch Optimization for Ride-Sharing

### Reference Paper Details
* **Title:** Optimizing Order Dispatch for Ride-Sharing Systems
* **Author(s):** Yubin Duan, Ning Wang, and Jie Wu
* **Published Year:** 2019
* **Publication:** proceedings of the 28th International Conference on Computer Communications and Networks (ICCCN 2019)
* **Document Link:** [https://ieeexplore.ieee.org/abstract/document/8847177](https://ieeexplore.ieee.org/abstract/document/8847177)
* **Document ID:** 8847177

### Technologies Used in Base Paper
* Geohashing (Spatial partition hashing)
* Bipartite Graph Matching Algorithms (Kuhn-Munkres/Hungarian algorithm variant)
* Network Simulation Tools (Python / MATLAB matching models)

### Key Objectives & Methodology
The paper addresses inefficiencies in urban ride-sharing order dispatch systems. It proposes a dispatch framework that broadcasts passenger requests to drivers within an iteratively expanding dispatch region:
* **Hybrid Matching Model:** Combines system-assigning (platform-driven optimization) and driver-grabbing (driver autonomy) strategies to balance passenger wait times and driver earnings.
* **Iterative Expansion Algorithm:** Dynamically expands the geographical search boundary for an unmatched order, moving from localized geohash regions outward to neighboring areas.

### Advantages
* **Optimized Throughput:** Ranks global efficiency instead of localized greedy nearest-neighbor matching.
* **Balanced Interests:** Jointly optimizes passenger wait times and driver earning potential.
* **Flexible Search Area:** Prevents order dropping in low-driver areas through dynamic search cell expansion.

### Disadvantages
* **No Real-Time Cloud Ingestion:** Does not model high-throughput data pipelines (like Kafka or WebSockets) for continuous GPS coordinate ingestion.
* **Lacks Scale Heuristics:** Graph matching complexity increases exponentially with high active-trip counts.
* **No Dynamic Surge Pricing:** Lacks integration with dynamic, supply-demand spatial pricing services.

---

## 2. ChainSight: Smart IoT Inventory and Predictive Supply Chain Platform

### Reference Paper Details
* **Title:** IoT Application for Smart Inventory Management System Based on RFID
* **Author(s):** Souhir Bousselmi, Moez Gannouni, and Kaïs Ouni
* **Published Year:** 2024
* **Publication:** IEEE Conference Publication (Indexed in IEEE Xplore 2025)
* **Document Link:** [https://ieeexplore.ieee.org/document/10919846](https://ieeexplore.ieee.org/document/10919846)
* **Document ID:** 10919846

### Technologies Used in Base Paper
* RFID (Radio Frequency Identification) tags and scanners
* Raspberry Pi 4 edge compute board
* MQTT communication protocol
* Relational database systems and web dashboard interfaces

### Key Objectives & Methodology
The paper focuses on solving accuracy and delay issues in warehouse inventories through automated, contactless scanning:
* **Contactless Tracking:** Uses Radio Frequency Identification (RFID) tags and scanners to track items as they move through warehouse entry and exit zones.
* **Edge-to-Centralized Transmission:** Details a configuration where edge scanners read RFID tags and transmit the raw IDs to a central database system for inventory updates.

### Advantages
* **Automated Scans:** Minimizes manual barcode scanning labor and eliminates human errors in tracking.
* **Edge-Driven Integration:** Lowers edge deployment costs using lightweight Raspberry Pi boards.
* **MQTT Efficiency:** Utilizes lightweight pub-sub MQTT protocol suitable for low-bandwidth networks.

### Disadvantages
* **Database Bottlenecks:** Direct edge-to-database writes create locks when multiple scanners fire simultaneously.
* **Telemetry Event Noise:** Lacks stream deduplication to filter duplicate reads from static tags resting near the scanners.
* **No Restocking Integration:** Lacks automated restocking triggering and predictive supply analytics.

---

## Comparative Literature Survey 

| Aspect | Case Study 1: RouteSync (Ride-Sharing) | Case Study 2: ChainSight (IoT Inventory) |
| :--- | :--- | :--- |
| **Base IEEE Paper** | "Optimizing Order Dispatch for Ride-Sharing Systems" | "IoT Application for Smart Inventory Management System Based on RFID" |
| **Author(s) & Year** | Yubin Duan, Ning Wang, Jie Wu (2019) | Souhir Bousselmi, Moez Gannouni, Kaïs Ouni (2024) |
| **Technologies Used** | Geohashing, Bipartite Graph Matching, Heuristics. | RFID Tags, Raspberry Pi 4, MQTT Protocol. |
| **Advantages** | Reduces deadhead mileage; increases global matching throughput. | High accuracy contactless counts; low edge deployment costs. |
| **Disadvantages** | Scales poorly at high concurrency; lacks surge pricing model. | Susceptible to RFID scan noise; creates database locks at high writes. |
| **Proposed Innovation** | Real-time WebSockets with local spatial-temporal H3 grids. | Window-based stream deduplication and Polyglot database persistence. |
| **Cloud Target** | AWS API Gateway, EKS (Kubernetes), Redis Geo, Kinesis. | AWS IoT Core, Managed Kafka (MSK), Amazon Timestream. |
