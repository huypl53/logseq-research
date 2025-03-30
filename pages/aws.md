> tags:: [[cloud computing]]

- Horizontal Scaling
  collapsed:: true
	- Create launch template
		- not subnet selection -> later in launch time
		- `User data` is a script run at startup
			- ```bash
			  #!/bin/bash
			  sudo yum update
			  sudo yum install httpd -y
			  sudo systemctl start httpd
			  echo "<h1> we're on the $(hostname -I)</h1>" | sudo tee /var/www/html/index.html
			  
			  ```
	- Create [[aws/EC2]] from launch template for testing only
	- Create [[aws/SG]] for the load balancer [[aws/EC2/ELB]] to be able to listen on port 80
	- Create [[aws/EC2/TG]]. Then attach that [[aws/EC2/TG]] to the current [[aws/EC2/ELB]], the load balancer will forward the requests to the [[aws/EC2/TG]]
	- Create [[aws/EC2/ASG]] with the created Launch Template, attach it to [[aws/EC2/TG]]
	- {{renderer excalidraw, excalidraw-2025-02-20-21-24-15}}
- [[aws/IAM]]
- deck:: aws
	- Common problems
	  collapsed:: true
		- application is accessible (timeout) #card
		  id:: 67b97621-cd78-4090-9d94-ad42a7bfcc4b
			- -> [[aws/SG]] issuse
		- "connection refused" -> application error/not launched
	- Recommendation
	  collapsed:: true
		- Should run `aws configure` on CLI: #card
		  id:: 67b97e15-ef4a-401e-94de-1cd14ec5db7c
			- ((67b96665-2d3f-463c-b4cd-4b9c1c50589a))
			-
- ## To-learn
	- LATER Access Control;
	  logseq.order-list-type:: number
	- LATER Amazon CloudFront;
	  logseq.order-list-type:: number
	- LATER Amazon CloudWatch;
	  logseq.order-list-type:: number
	- LATER Amazon DynamoDB;
	  logseq.order-list-type:: number
	- LATER Amazon Elastic Block Store (Amazon EBS);
	  logseq.order-list-type:: number
	- LATER Amazon Elastic Compute Cloud (Amazon EC2);
	  logseq.order-list-type:: number
	- LATER Amazon Elastic MapReduce (Amazon EMR);
	  logseq.order-list-type:: number
	- LATER Amazon Redshift;
	  logseq.order-list-type:: number
	- LATER Amazon Relational Database Service (Amazon RDS);
	  logseq.order-list-type:: number
	- LATER Amazon Resource Names (ARN);
	  logseq.order-list-type:: number
	- LATER Amazon Route 53;
	  logseq.order-list-type:: number
	- LATER Amazon Simple Storage Service (Amazon S3);
	  logseq.order-list-type:: number
	- LATER Amazon Simple Queue Service (Amazon SQS);
	  logseq.order-list-type:: number
	- LATER Authentication & Authorization;
	  logseq.order-list-type:: number
	- DONE Availability Zones;
	  logseq.order-list-type:: number
	- LATER AWS CloudFormation;
	  logseq.order-list-type:: number
	- LATER AWS CloudTrail;
	  logseq.order-list-type:: number
	- LATER AWS CodeCommit;
	  logseq.order-list-type:: number
	- LATER AWS CodeDeploy;
	  logseq.order-list-type:: number
	- LATER AWS Direct Connect;
	  logseq.order-list-type:: number
	- LATER AWS Identity and Access Management (AWS IAM);
	  logseq.order-list-type:: number
	- LATER AWS Key Management Service (AWS KMS);
	  logseq.order-list-type:: number
	- LATER AWS Storage Gateway;
	  logseq.order-list-type:: number
	- LATER Cloud Concepts;
	  logseq.order-list-type:: number
	- LATER Compliancy, Governance, Identity & Privacy;
	  logseq.order-list-type:: number
	- DONE Elastic IP (EIP);
	  logseq.order-list-type:: number
	- LATER Inbound Data Traffic & Outbound Data Traffic;
	  logseq.order-list-type:: number
	- LATER Input/Output operations Per Second (IOPS)
	  logseq.order-list-type:: number
	- LATER Public & Private Cloud;
	  logseq.order-list-type:: number
	- LATER Service Level Agreement (SLA);
	  logseq.order-list-type:: number
	- LATER Software as a Service (SaaS);
	  logseq.order-list-type:: number
	- DONE Virtual Private Clouds (VPC);
	  logseq.order-list-type:: number
- ## Q&A
  collapsed:: true
	- Solution Architect Associate Certification SAA-C03
	- deck:: aws
		- **Elastic Network Interface (ENI):**
			- **What is an Elastic Network Interface (ENI) in AWS?** #card
			  id:: 67ce41ba-3739-4759-a729-bcc706b799e5
				- *Answer:* An ENI is a logical component within a Virtual Private Cloud (VPC) that represents a virtual network card. It can include attributes such as a primary private IPv4 address, one or more secondary private IPv4 addresses, one Elastic IP address per private IPv4 address, one public IPv4 address, one or more IPv6 addresses, one or more security groups, a MAC address, and a source/destination check flag.
			- **How can ENIs be utilized for failover scenarios in AWS?** #card
			  id:: 67ce41ba-6f91-476f-a39b-37d6b22aa1d2
				- *Answer:* ENIs can be created independently and attached or detached from EC2 instances on the fly. In failover scenarios, you can move an ENI from one instance to another within the same Availability Zone (AZ), allowing the new instance to assume the network configurations of the original instance seamlessly.
			- **Are ENIs restricted to specific Availability Zones (AZs)?** #card
			  id:: 67ce41ba-4727-41c4-8132-e67606067bcc
				- *Answer:* Yes, ENIs are bound to a specific AZ. They can only be attached to instances within the same AZ.
		- **EC2 Hibernate:**
		- **What is the Hibernate feature in EC2, and when would you use it?** #card
		  id:: 67ce4204-439c-4225-9fdc-7bee534e708c
			- *Answer:* The Hibernate feature allows you to pause your EC2 instance by saving its in-memory state (RAM) to the root EBS volume. When you start the instance again, the operating system and applications resume from their previous state, reducing the time required to return to operation. This is particularly useful for instances that require long initialization times or have applications that maintain a complex state in memory.