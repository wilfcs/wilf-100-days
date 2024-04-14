TikTok Architecture:

Scale: 10M daily active users, 100K creators uploading 200K 1MB videos per day
Microservices: Ingest service, video serving service, databases
Upload: HTTPS protocol, parallel processing/chunking for transcoding
Storage: S3 for video files, NoSQL for metadata, SQL for user data
CDN for video distribution across regions for low latency
Caching for hot/trending videos at CDN level
Decouple ingestion rate from processing rate using queues
High availability via replication across zones, fault tolerance
Cost estimation and networking protocols required deeper knowledge