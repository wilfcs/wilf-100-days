HLD->

Anomaly Detection in Server Systems
Detecting anomalies is important for managing the health of server systems and alerting when something goes wrong.
Servers publish metrics like CPU usage, memory, etc. to an analytical engine that detects anomalies in this data.
It's better to allow some false positives than miss real alerts (false negatives).
A service mesh with sidecars can help standardize how services publish metrics.
Common anomaly detection techniques:
Setting static thresholds (high/low limits) - prone to errors, doesn't account for trends
Dynamic high/low pass filters that adapt to trends
Using historical data to define "normal" bands
Machine learning models like Isolation Trees
Isolation Trees work by trying to isolate anomalies with a minimum number of data partitions, treating highly isolated data points as anomalies.
Live Video Streaming Systems
Need to ingest the live video stream, often using RTMP protocol over TCP for reliability.
The video is transformed into multiple resolutions/codecs by a transformation service assigning jobs to workers.
Workers publish results to a distributed file storage.
Subscribers pull these transformed video streams to serve clients.
Live streaming has tight latency requirements compared to video-on-demand.
Migrating from Monoliths to Microservices
Monoliths have all code in one repo, microservices split the codebase into separate services.
Advantages of microservices:
Separation of concerns - easier to make changes to a single service
Independent deployment of services
Better scalability of teams
Monolith advantages: Simpler inter-module communication, harder to make breaking changes
Migration strategy:
Separate out new features into their own services
Setup service contracts/clients for inter-service communication
Routing layer like an API gateway
Streamlined deployment processes
Async messaging or request/response based communication between services
Centralized logging
Data storage: Each service should "own" the data it is responsible for
Don't splinter services too much, condense related responsibilities
Consider infrastructure costs of a microservices architecture
Team size considerations: <2 services/person for startups, 1 service/person for mid-size, 2-4 people/service for large orgs