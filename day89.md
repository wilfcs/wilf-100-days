Number of Good Paths

Collect Coins in a Tree

Modify Graph Edge Weights

Reconstruct Itinerary

Longest Increasing Path in a Matrix

Remove K Digits

HLD ->

1. Rate limiting is used to prevent servers from being overwhelmed by too many requests, which can cause a "thundering herd" of bison crushing the system.

2. When one server goes down, its load is distributed to the remaining servers. If those servers cannot handle the additional load, it can lead to a cascading failure where servers keep crashing one by one.

3. To mitigate this, requests can be queued up to a maximum capacity for each server. Once that capacity is reached, additional requests are rejected temporarily until more capacity becomes available.

4. For events like Black Friday sales, pre-scaling (adding more servers ahead of time) or auto-scaling can help handle increased traffic.

5. Batch processing of jobs (like sending notification emails) in smaller chunks over time, instead of all at once, prevents overwhelming the system.

6. For viral/popular content, approximating non-critical metadata (like view counts) instead of querying the exact value can reduce load.

7. Caching, gradual deployments, and careful data coupling between services are other techniques to improve performance and handle more load.

8. There are trade-offs between performance optimizations like caching/coupling and factors like data consistency that require careful consideration.

