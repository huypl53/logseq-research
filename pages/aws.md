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
-
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