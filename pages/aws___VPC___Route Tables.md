## Subnet association
	- A subnet is linked to one route table at a time.
- ## Targets
	- **Internet Gateway (IGW)**: For internet access (igw-xxxx).
	- **NAT Gateway**: For private subnets to reach the internet (nat-xxxx).
	- **VPC Peering**: Connect to another VPC (pcx-xxxx).
	- **Virtual Private Gateway**: For VPN/Direct Connect (vgw-xxxx).
	- **Local**: Traffic within the VPC (automatic, e.g., 10.0.0.0/16 → local).