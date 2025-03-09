- Strategies
	- deck:: aws
	- **Cluster**: #card
	  id:: 67cdb8bf-1a72-46c6-9259-84b89da469ab
		- **Purpose**: High performance (e.g., low-latency, high-throughput apps like HPC).
		- **How**: Packs instances **close together in a single AZ** on the same underlying hardware.
		- **Pros**: Low network latency, high bandwidth (up to 10 Gbps intra-group).
		- **Cons**: Limited to one AZ; higher risk if hardware fails, all instances fails.
	- **Spread**: #card
	  id:: 67cdb8bf-e4a4-4ec1-a8bf-af25a57e8a1f
		- **Purpose**: High availability (e.g., critical apps needing isolation).
		- **How**: Places instances on **distinct hardware racks,** each with its own power and network, **across multiple AZs** (max 7 instances per group per AZ).
		- **Pros**: Minimizes correlated failures.
		- **Cons**: Limited instance count; higher latency between instances.
	- **Partition**: #card
	  id:: 67cdb8bf-31c3-4fc5-94b1-970bbb2c5c79
		- **Purpose**: Balance performance and availability (e.g., big data frameworks like Hadoop).
		- **How**: Divides instances **into partitions (up to 7 per AZ)**, each on **separate hardware rack**s. **Instances in one partition stay isolated from others.**
		- **Pros**: Fault isolation with decent density; supports multiple AZs. **Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)**
		- **Cons**: Less tight packing than Cluster; partition size limits apply.
- Steps:
	- Create EC2 instance
	- Attach it to a Placement Group