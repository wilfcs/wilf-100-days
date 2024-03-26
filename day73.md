
Introduction to System Design
- An algorithm/code running on a computer is a server
- It takes requests from clients and sends back responses
- As the number of clients grow, need to scale system to handle more requests

Scaling
- Vertical Scaling: Upgrade to bigger/faster machine
    - Pros: No load balancing, fast interprocess communication, data consistency
    - Cons: Single point of failure, hardware limits
- Horizontal Scaling: Add more machines  
    - Pros: Resilience, scales linearly
    - Cons: Need load balancing, network calls are slow, data consistency issues

Load Balancing
- Evenly distributes load across multiple servers
- Use hashing techniques to map requests to servers
- Consistent Hashing avoids re-mapping everything when adding/removing servers

Microservices
- Break system into smaller services focused on single responsibility  
- Allows scaling different parts at different rates
- Enables fault isolation

High Availability
- Distribute system across multiple locations/data centers
- Local servers provide faster response times
- Load balancer routes requests intelligently

Decoupling
- Separate components like web servers, databases, caches
- Allows components to scale independently  
- Improves flexibility and extensibility

Monitoring & Metrics
- Log events to understand system behavior
- Collect metrics on performance, errors, etc.
- Use to identify bottlenecks, failures

Message Queues
- Asynchronous request handling using queues
- Persists requests, assigns to workers, re-queues on failure
- Provides load balancing, buffering at peaks

Overall themes: scalability, availability, decoupling, monitoring