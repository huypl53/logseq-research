- > Elastic Network Interface: logical component in a VPC that represents a **virtual network card**
- you can create ENI independently and attach them on the fly (move them) on EC2 instances for failover
- be bound to a specific AZ
- Notes
	- deck:: aws
	- ENI attributes: #card
	  id:: 67cdbda9-8aa7-4274-b619-96472404a211
		- On EC2 has a default primary private IPv4, one or more secondary IPv4
		- One Elastic IP (IPv4) per private IPv4
		- One Public IPv4
		- One ore more [[aws/SG]]
		- A MAC address
	- Usages: #card
	  id:: 67cdc087-1d45-4e05-b0be-bee4728a5926
		- each private IPv4 associated with **an ENI can have an Elastic IP** -> consistent public IP even if underlying instance changes
		- move ENIs between instances in case of failures -> in case of instance failure, no need to change DNS or client configuration
		- ENIs have own SG -> different SGs on same instance due to ENI