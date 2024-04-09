DSA questions solved today->

Water and Jug Problem

House Robber III

Binary Tree Cameras

Longest ZigZag Path in a Binary Tree

Coin Change

Maximum Compatibility Score Sum

HLD->

Master-Slave Architecture for Data Replication

1. Problem: Single database is a single point of failure

2. Solution: Replicate data to a slave/standby database
    - Master database handles all write operations
    - Slave databases are read-only copies

3. Asynchronous vs Synchronous Replication
    - Asynchronous: Slave updates lazily, may have stale data
    - Synchronous: Slave updated immediately after each write to master

4. Write Propagation 
    - Typically, slaves cannot directly accept writes 
    - Writes flow from master to slaves to maintain consistency
    - Master-master setup is prone to split-brain problem

5. Adding an Arbitration Node
    - Third node can solve split-brain by arbitrating between masters
    - Nodes agree on state via consensus protocols like 2PC, 3PC, Paxos
    - Multi-version concurrency control (MVCC) maintains data versions

6. Sharding 
    - Split data across masters by range (e.g. user id)
    - Each shard has a master-slave setup
    - Reduces failover blast radius 

7. Advantages
    - Data redundancy and failover
    - Read scalability by adding slaves
    - Separating reads and writes

Key Points:
- Master source of truth for writes
- Slave read-replicas stay up-to-date 
- Consensus protocols for coordinating state
- Sharding reduces coordination overhead