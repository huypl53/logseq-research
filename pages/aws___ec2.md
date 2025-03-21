- Elastic IP
	- deck:: aws
	- when EC2 instance reset, **its public IP changes** -> attach instance to an Elastic IP #card
	  id:: 67cdb4fa-bf78-4085-8a2a-31495a815ee8
		-
	- **try to avoid using Elastic IP**: #card
	  id:: 67cdb553-98ba-4df9-9dff-49de709b1d46
		- They often reflect poor architectural decisions
		- Instead, use **a random public IP and register a DNS name** to it
		- use a [[aws/EC2/ELB]]
- ## Q&A
	- deck:: aws
	- can use an AMI in `us-east-1` to launch an EC2 instance in any AWS region? #card
	  id:: 67ce5907-4603-4ec1-8203-2a59e171f494
		- AMIs are built for a specific AWS Region, they're unique for each AWS Region.
		- You can't launch an EC2 instance using an AMI in another AWS Region, but
		   you can copy the AMI to the target AWS Region and then use it to create
		   your EC2 instances.
	- Which of the following EBS volume types can be used as boot volumes when you create EC2 instances? #card
	  id:: 67ce597e-45c6-4584-96f2-5a7eda4d7ccd
		- When creating EC2 instances, you can only use the following EBS volume 
		  types as boot volumes: gp2, gp3, io1, io2, and Magnetic (Standard).
	- You are running a high-performance database that requires an IOPS of 310,000 for its underlying storage. What do you recommend?
		- EBS `io1`: 60000 IOPS
		- EC2 Instance Store: 320.000 IOPS - CHECKED
			- to address data failure on machine termination,
				- set up a replication on another EC2 with an Instance Store to have a standby copy
				- set up backup mechanism
		- EBS `io2` Block Express drive: 250000 IOPS