Messaging Queues:
Analogy of a pizza shop taking orders asynchronously and processing them as a queue
Orders are taken without waiting to make the pizza first
Customers are given order confirmations upfront
Orders are stored in a queue and pizzas made in that order
Use a persistent queue stored in a database to handle server failures
If a server goes down, its unprocessed orders can be rerouted to other servers
Database acts as durable storage for the queue
Load balancing and heartbeat monitoring for failover
Heartbeat monitor checks if servers are alive
If a server fails, its pending orders are retrieved from database
Load balancer redistributes these orders across remaining servers using techniques like consistent hashing to avoid duplicates
Message/Task queues encapsulate complexities:
Assignment of tasks to workers
Persistence and durability
Load balancing strategies
Notifications and acknowledgments
Retry and dead letter handling
Examples: RabbitMQ, Amazon SQS, Apache Kafka
Monolith vs Microservices Architecture:
Monolith
Single codebase/application hosting all functions
Advantages: Simple for small teams, fewer dependencies, faster in-process communication
Disadvantages: Tight coupling, hard to onboard new devs, disruptive deployments
Microservices
Application composed of small services handling specific capabilities
Services interact via lightweight APIs (HTTP, gRPC, etc.)
Own data store per service
Advantages: Independent scaling, parallel dev, isolated failures, assignable scope
Disadvantages: Increased complexity, potential over-engineering, intersystem communication
Trade-offs evaluated based on team size, application scale, requirements
Database Sharding:
Sharding partitions data rows horizontally across multiple database servers
Based on a shard key like user id or location id
Improves read/write performance by reducing data volume per shard
Issues:
Joins across shards complex and expensive over the network
Inflexible and static shard count
Solutions:
Consistent hashing distributes data across shards based on hash of key
Hierarchical sharding splits shards into sub-shards for dynamic resizing
Indexes on sharded databases improve queries
Master-slave architecture per shard for fault tolerance
Master handles writes, replicated to read-only slaves
Slaves handle read load
If master fails, a slave is elected as new master