RouteSync – High-Level Design
Purpose
RouteSync is a cloud-based real-time ride-hailing and dispatch optimization platform designed to efficiently connect passengers with drivers using dynamic spatial-temporal matching.

Core Problem
Legacy dispatch systems rely on simple, greedy proximity matching (nearest available driver). This approach fails to react to real-time spatial-temporal demand shifts, leading to long passenger wait times, high driver deadhead mileage, unbalanced driver supply across city zones, and lost earnings during peak traffic.

Proposed Architecture
Rider App ↓ API Gateway / WebSocket Gateway ↓ Location Ingestion Service ↓ Real-Time Driver Geospatial Cache ↓ Batched Match Optimizer ↓ Dispatch Region Expansion Engine ↓ Surge Pricing Service ↓ Ride Match Lifecycle Service ↓ Driver Assignment/Rider Notification ↓ Analytical Time-Series Store ↓ Operational PostgreSQL Database

Major Components
Managed API & WebSocket Gateway
Trip Lifecycle Microservice
Ingestion Location Service
Batched Match Optimizer
Geospatial Region Expansion Engine
Surge Pricing Service
Demand Forecasting ML Service
Notification and Dispatch Dispatcher
Geospatial Memory Cache
Analytical Database Store
Admin Dashboard Console
Proposed Innovation Direction
The system will investigate an Iterative Dispatch-Region Expansion and Batched Matching Model. The important research direction is to determine how to dynamically group and match passenger requests with drivers within expanding geohash/H3 boundary cells, ensuring global system balance and driver fairness rather than static nearest-neighbor assignment.

The system will evaluate spatial-temporal occupancy density to adjust matching batch windows and geographical search radius dynamically during demand surges.

Cloud Deployment
The architecture is designed as a cloud-native service using managed WebSocket API gateways, containerized microservices on Kubernetes, high-throughput event streaming, in-memory geospatial caching, relational databases, and predictive machine learning endpoints.

Expected Outcome
A prototype capable of matching riders and drivers in real time, expanding search zones dynamically, and adjusting pricing based on simulated urban demand hotspots.