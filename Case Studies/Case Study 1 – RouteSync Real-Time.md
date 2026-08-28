Case Study 1 – RouteSync Real-Time Geospatial Dispatch Optimization for Ride-Sharing
1. Introduction
Urban commuter and ride-sharing operations are highly dynamic, requiring rapid matching of riders and drivers. While basic location services and routing APIs are common in the industry, traditional dispatching systems fail to optimize supply-demand matches dynamically over space and time, leading to inefficiencies during peak traffic and sudden demand spikes.

2. Core Problem
Legacy dispatch systems rely on simple, greedy proximity matching, assigning the nearest available driver to a rider. This approach fails to react to real-time spatial-temporal demand shifts, resulting in longer average passenger wait times, unbalanced driver supply across city zones, increased driver idle mileage (deadheading), and poor driver earnings.

The problem to investigate is: How can a cloud system dynamically optimize driver-passenger matches across expanding geospatial zones in real-time, rather than relying on static, single-point proximity matching?

3. Proposed Solution
RouteSync is proposed as a cloud-native, real-time ride-hailing and dispatch optimization platform. Instead of matching drivers individually on simple distance, it evaluates the global supply-demand state using:

Real-time GPS coordinate streams
Geospatial grid indexing (H3/geohash)
Iterative dispatch-region expansion
Dynamic surge-pricing ratios
Near-term demand prediction models
The system produces batch-optimized driver-passenger assignments.

4. Proposed Technical Innovation
The project will investigate an Iterative Dispatch-Region Expansion and Batched Matching Model. The important research direction is to determine how to dynamically group and match requests within expanding geospatial bounds to minimize global wait times and idle mileage, rather than immediately matching the closest driver.

Example:

Rider request → spatial-temporal batching → initial local geohash search → no available drivers → iterative H3 region expansion → multi-objective match optimization → optimized driver assignment.

5. Cloud Deployment
The platform can be deployed using a cloud API gateway, WebSocket gateway, real-time location service, dispatch engine running on managed Kubernetes, in-memory geospatial store (Redis Geo), time-series event logger, dynamic pricing service, relational database (PostgreSQL with PostGIS), and push-notification queues.

6. Expected Impact
RouteSync could help operators reduce aggregate passenger wait times, minimize driver deadhead mileage, maximize vehicle utilization rates, and dynamically balance driver supply across high-demand urban hotspots.

7. Case Study Takeaway
The project investigates a shift from “static proximity-based matching” to “real-time, batch-optimized spatial-temporal dispatching.”