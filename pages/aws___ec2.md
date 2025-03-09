- Elastic IP
	- deck:: aws
	- when EC2 instance reset, **its public IP changes** -> attach instance to an Elastic IP #card
	  id:: 67cdb4fa-bf78-4085-8a2a-31495a815ee8
		-
	- **try to avoid using Elastic IP**: #card
	  id:: 67cdb553-98ba-4df9-9dff-49de709b1d46
		- They often reflect poor architectural decisions
		- Instead, use **a random public IP and register a DNS name** to it
		- use a [[aws/EC2/LB]]