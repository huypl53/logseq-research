deck:: aws

- ## Attributes #card #incremental
  id:: 67e89c92-f1a2-48e9-9bc3-f044bb11e18b
	- shared storage volume: replication + self healing + auto expanding
	- master + up to **15 Aurora Read Replicas** serve reads
	- Backtrack: restore data at **any point of time without using backups**
	- Auto-scaling replicas
- ## Aurora endpoints #card #incremental
  id:: 67e89d0e-a7e9-4c1d-924a-22c0648203bb
	- Default/cluster endpoint (for writes): -> primary instance (read + write)
	- reader endpoint: load-balances across all read replicas
	- Custom endpoint: user-defined subset of instances
- Auto-Scaling #card #incremental
  id:: 67e8a007-322b-4b2d-a9b5-43d92d2901cf
	- based on current replicas' CPU usage, auto scale
- ## Types #card #incremental
  id:: 67e8a8d7-5c1f-46d8-8d96-24bb56d96816
	- Aurora serverless: **automatically scales compute capacity** based on workload demand, without managing instances.
	- Aurora global database: low-latency global, disaster recovery
		- **Primary Cluster**: **One writable Aurora cluster** in a primary region
		- **Secondary Clusters**: Up to 5 read-only clusters in other regions
			- up to 16 read replicas per secondary region
	- Aurora machine learning:
		- ML-based predictions via SQL
		- e.g: fraud detection, ads targeting, sentiment analysis