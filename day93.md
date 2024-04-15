System Design - Online Judge for Coding Contests
- Covers designing a remote code execution engine for online coding contests/interviews
- Key components:
  - Message queue to handle burst of submission requests asynchronously instead of overloading system
  - Store code files in distributed file system like S3
  - Execute code submissions in containers/VMs for security and isolation
  - Use a job scheduler service to process code executions based on available capacity
  - Store results (accepted/wrong answer) in database
  - Separate service to fetch/display results to users

Data Consistency in Distributed Systems  
- Explains the CAP theorem's Consistency aspect for distributed data stores
- Having a single data copy is a single point of failure 
- Replicating data across multiple servers improves availability but raises consistency challenges
- Techniques for achieving consistency:
  - Leader-follower model where leader handles writes, propagates to followers
  - Two-phase commit protocol across replicas for atomic transactions
  - Trade-off between strong consistency (blocking reads/writes) and availability
- Eventual consistency is an alternative approach
