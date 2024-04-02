Caching:

1. Caching is used to speed up responses to clients by avoiding costly database queries or network calls.
2. Two main use cases for caching:
   a. Caching commonly accessed data (e.g. user profiles) to avoid database queries
   b. Caching computationally expensive results (e.g. averages) to avoid repeated calculations
3. Cache placement options:
   a. In-memory cache on server (fast but not resilient if server fails)
   b. Distributed global cache (e.g. Redis) shared across servers (slower but more resilient)
4. Cache policies determine what data gets loaded/evicted, e.g:
   a. Least Recently Used (LRU)
   b. Sliding window policies
5. Write policies: 
   a. Write-through (update cache first then database)
   b. Write-back (update database first then cache)
   c. Hybrid approaches are often used
6. Consistency considerations - caching can lead to stale data being served

API Design:

1. An API (Application Programming Interface) specifies how external systems interact with your code/service.
2. API definition includes parameters, return types, errors, and the action being performed.
3. Proper naming of APIs is crucial to convey their intent accurately.
4. Avoid APIs with side-effects - an API should do one thing well.
5. Atomicity may be required for related operations in some use cases.
6. Pagination and fragmentation can handle large responses.
7. Consider consistency requirements - caching can lead to stale data being returned.
8. Service degradation techniques like reducing response sizes can handle high load.
9. API design is critical for developer experience and maintainability.

The key themes covered are the use of caching to improve performance, factors influencing cache design and policies, API design principles, handling large responses, data consistency considerations, and resilience techniques like service degradation.