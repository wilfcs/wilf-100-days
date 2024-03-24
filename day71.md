Clustering in DBMS:
- Database Clustering (making Replica-sets) is the process of combining more than one servers or instances connecting a single database.
- Sometimes one server may not be adequate to manage the amount of data or the number of requests, that is when a Data Cluster is needed.
- Replicate the same dataset on different servers.
- If one node fails, the request is handled by other nodes.l;kwejrlksdjl flskajn flj aslf jasdk fjlk sdfljas jflsa fll ksd
- Advantages:
	- Data Redundancy: Clustering of databases helps with data redundancy, as we store the same data at multiple servers. Don’t confuse this data redundancy as repetition of the same data that might lead to some anomalies. The redundancy that clustering offers is required and is quite certain due to the synchronisation. In case any of the servers had to face a failure due to any possible reason, the data is available at other servers to access.
	- Load balancing
	- High availability

Partitioning & Sharding in DBMS (DB Optimisation)
- How to deal with big data now? There are two methods that we can apply that we already know. One is vertically scaling up. But that is very costly but when the data is very big it doesn't do any optimization. It doesn't make your dbms fast. Another is clustering. But clustering increases delay when data is big because there is one master node that has to do the copying of the data. So one better way to do optimization is partioning.
- A big problem can be solved easily when it is chopped into several smaller sub-problems. That is what the partitioning technique does. It divides a big database containing data metrics and indexes into smaller and handy slices of data called partitions. The partitioned tables are directly used by SQL queries without any alteration. Once the database is partitioned, the data definition language can easily work on the smaller partitioned slices, instead of handling the giant database altogether. This is how partitioning cuts down the problems in managing large database tables.
- Partitioning is the technique used to divide stored database objects into separate servers. Due to this, there is an increase in performance, controllability of the data. We can manage huge chunks of data optimally. This is a horizontal scaling technique.
- There are two types of Partioning:
	- Vertical Partitioning: Slicing relation horizontally / row-wise and storing them in different servers. I.e. We store different columns in different DB. 
	- Horizontal Partitioning: Slicing relation horizontally / row-wise. Independent chunks of data tuples are stored in different servers.
- The different DB where these data are kept are called shards
- Sharding:
	- Technique to implement Horizontal Partitioning.
	- The fundamental idea of Sharding is the idea that instead of having all the data sit on one DB instance, we split it up and introduce a Routing layer so that we can forward the request to the right instances that actually contain the data.
	  