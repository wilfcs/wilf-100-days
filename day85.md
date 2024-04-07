Event-Driven Systems

1. **Introduction**
   - Event-driven architecture is different from the traditional request-response architecture.
   - In event-driven systems, services communicate by sending events, not direct requests.
   - When a service's state changes, it publishes an event to an event bus.
   - Interested subscribers (other services) consume the event and react accordingly.

2. **Advantages**
   - High availability: Even if a service goes down, others can still function with stored events.
   - Better consistency: Services store events in local databases, reducing inconsistencies.
   - Time travel debugging: Event logs allow replaying events to debug issues at any point.
   - Smooth service replacements: New services can replay events to achieve consistency.
   - Transactional guarantees: Events can be delivered at least once or at most once.
   - Capturing intent: Event logs capture the intent behind data changes, useful for new services.

3. **Disadvantages**
   - External service dependencies: Handling external responses based on time is challenging.
   - Lack of fine control: Difficult to control event handling timings and priorities.
   - Event filtering: Hard to decide which events to publish and which services should consume them.
   - Replay inefficiencies: Replaying all events from start is impractical; diff-based or undo approaches have limitations.
   - Reasoning flow: Difficult to reason about the program flow across multiple services.
   - Migration challenges: Migrating parts of the system to request-response is not straightforward.

4. **Use Cases**
   - Git, React, Node.js, and gaming systems use event-driven architectures.
   - Useful for systems that require high availability and eventual consistency.

NoSQL Databases

1. **Introduction**
   - NoSQL databases store data in a flexible, schema-less way, unlike SQL databases.
   - Data is stored as JSON-like documents, making insertions and retrievals efficient.

2. **Advantages**
   - Schema flexibility: Easy to add or modify fields without altering the schema.
   - Horizontal partitioning: Built-in support for scaling out by partitioning data across nodes.
   - Availability: NoSQL databases prioritize availability over strict consistency.
   - Aggregations: Designed for efficient data aggregations and metrics calculations.

3. **Disadvantages**
   - Consistency issues: No guaranteed ACID properties, making transactions challenging.
   - Read inefficiency: Reading specific columns requires scanning entire documents.
   - Lack of implicit relationships: No built-in support for enforcing relationships like foreign keys.
   - Join difficulties: Joining data from multiple collections is a manual process.

4. **Use Cases**
   - Suitable when data is write-heavy, schema is flexible, and availability is prioritized over consistency.
   - Not recommended for systems with strict transactional requirements or complex join operations.

5. **Cassandra Architecture**
   - Requests are hash-partitioned across nodes in the cluster.
   - Data is replicated across nodes for redundancy and load balancing.
   - Quorum consensus is used to resolve conflicts and ensure availability.
   - Data is stored in immutable Sorted String Tables (SSTables), which are periodically compacted.

6. **Key Concepts**
   - Hashing for data distribution and replication across nodes.
   - Quorum consensus for conflict resolution and availability guarantees.
   - SSTables and compaction for efficient storage and space optimization.

