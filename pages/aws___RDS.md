- **RDS instances are managed services**, and AWS provides a **DNS endpoint** instead of a fixed IP.
- The private IP **may change** when RDS restarts, so AWS prefers you use the **endpoint**.
- deck:: AWS
- ## Attributes: #card #incremental
  id:: 67e7a824-7073-479e-9a48-27dbd498b007
	- within AZ, cross AZ or Cross Region
	- **Storage backed by EBS**
	- Auto Vertical/Horizontal Scaling
	- Read Replica is free in same AZ with primary db, but **requires charge if cross AZ**
	- **Continuous backups and restore** to specific timestamp
- Async Replica #card
  id:: 67e7a824-105b-492d-b9bf-489b4ca753d3
	- **Replication Lag:** time for updates to reach the replica
	- App must **update conn string** to leverage read replicas
	- RDS Read Replicas: **same AZ -> free**
- Sync Replica #card
  id:: 67e7a824-408d-4b6d-ac8c-99a766b38f20
	- Multi AZ for **Disaster Recovery**
	- One **DNS name** - auto app failover to standby
	- **not used for scaling**
	- e.g: **standby** in Multi-AZ setup/modification
- |      Aspect      | RDS Replica (Read Replica) |   RDS Standby (Multi-AZ)  |
  |:----------------:|:--------------------------:|:-------------------------:|
  | Purpose          | Read scaling               | High availability         |
  | Replication      | Asynchronous               | Synchronous               |
  | Read Access      | Yes (readable)             | No (standby only)         |
  | Failover         | Manual promotion           | Automatic (60–120s)       |
  | Location         | Same/different AZ/region   | Different AZ, same region |
  | Setup            | Explicitly created         | Auto with Multi-AZ        |
  | Data Consistency | Eventual                   | Immediate (zero RPO)      |
  | Cost             | Extra instance             | Part of Multi-AZ fee      |
- ## Backup
	- Auto backup -> restore
		- daily full backup
		- transaction logs are backed-up every 5 minutes
	- Manual snapshot -> create new DB
		- manually trigger
		- manual retention setup
	- Cloning: faster than snapshot & restore
- ## Security #card #incremental
  id:: 67e8ae26-f151-4736-84ca-ed7e7bbbbf4d
	- encryption:
		- must be defined at launch time
		- to encrypt an un-encrypted db, take db snapshot -> **restore as encrypted**
	- IAM authentication: instead of username/password
	- Security Groups
	- **No SSH available except on RDS custom**
- ## Proxy #card #incremental
  id:: 67e8aefa-b41a-4251-a4d7-9d58c49fc1b9
	- Reduces the number of **direct database connections by pooling and reusing them**. (e.g: thousands of instant lambda function require connection )
	- **Availability****: Maintains connections during failovers**, reducing downtime (Proxy **preserves sessions transparently**)
	- Integrates with **AWS IAM and Secrets Manager** for credential management.
	- Improving database efficiency by reducing the **stress on database resources** (e.g., CPU, RAM) and **minimize open connections (and timeouts)**
	- RDS Proxy is **never** **publicly accessible (only from VPC)**
- ## Practice
	- mysql
	- auto SG is set
	- use sqlectron to connect to db
	- create replica
	- take snapshot; restore to point in time