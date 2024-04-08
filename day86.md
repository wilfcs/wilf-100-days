Log-Structured Merge Trees for Scaling Writes in Databases

1. Log for Fast Writes
- Use an append-only log data structure (like a linked list) for fast write operations (O(1))
- Server appends new records to in-memory log 

2. Sorted Tables for Fast Reads  
- In background, merge and sort log records into chunks/sorted string tables
- Sorted tables allow fast reads using binary search (O(log n))

3. Compaction
- Merge multiple sorted string tables into larger sorted tables
- Reduces number of tables to search for reads

4. Bloom Filters
- Create bloom filters on sorted tables to quickly check if record exists
- Avoids unnecessary searches, reduces false positives

5. Tune Merge Policy  
- Merge frequency and sorted table sizes tuned for write/read performance  
- Size limits control read overhead vs merge overhead

Key Advantages:
- Fast writes by appending to log  
- Fast reads on sorted tables created in background
- I/O minimized by batching writes and merges

Service Discovery and Health Checks in Microservices

1. Health Checks
- Health service monitors liveness of services by sending heartbeat messages
- Services must respond in time to be considered alive

2. Two-Way Heartbeats  
- Services also send reverse heartbeats to health service
- Avoids zombie services that are unresponsive but running

3. Service Discovery
- Services register IP addresses and ports with load balancer 
- Load balancer persists snapshot of active service instances

4. Load Balancer
- Routes requests to healthy service instances based on snapshot
- Can cache snapshot to avoid querying for each request  

5. Notifications
- Health service notifies load balancer of changes in service liveness
- Load balancer updates snapshot, stops routing to failed instances

Key Points:
- Reliable detection of service liveness
- Central registry of active service instances
- Ability to dynamically update service routing

