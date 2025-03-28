- > Auto Scaling Group: handles *instance scaling and lifecycle*, integrating with [[aws/EC2/TG]] for traffic routing.
- deck:: aws
- be able to create new instance from provided Launch template
- add/remove EC2 in [[aws/EC2/TG]] obeyed the defined capacity (desired, min, max)
- like other nodes in AWS, [[aws/ec2/asg]] need to attach to a [[aws/SG]] for publishing port 80
- manual config
- dynamic config
	- use [[aws/CloudWatch/Alarms]], which trigger action in [[aws/EC2/ASG]] to add/remove unit capacity. Check result in history
- ## Scenarios:
	- **Dynamic Workload**: Your app’s traffic fluctuates (e.g., e-commerce site during sales), requiring automatic addition/removal of instances.
	- **High Availability**: You want instances across multiple AZs, with ASG replacing failed ones and ELB balancing traffic.
	- **Cost Efficiency**: You want to scale down instances during low demand to save costs, while ensuring ELB only sends traffic to active instances.
	- **Simplified Management**: You prefer automated instance lifecycle (launch, terminate) tied to ELB health checks.
- ## Policy #card #incremental
  id:: 67dd2ebd-4dc9-4053-ac3c-8ad3a81215bb
	- Dynamic Scaling:
		- Target tracking scaling: e.g:  I want the average ASG CPU to stay at around 40%
		- Simple/Step Scaling: [[aws/CloudWatch]] alarm trigger
	- Scheduled Scaling
	- Predictive scaling: continuously forecast load and schedule scaling ahead
- ## Metrics
	- CPUUtilization: Average CPU utilization across your instances
	- RequestCountPerTarget: to make sure the number of requests per EC2 instances is stable
	- Average Network In / Out (if you’re application is network bound)
	- Any custom metric (that you push using CloudWatch)
- ## Scaling cooldown
	- During cooldown period, ASG will not launch or terminate additional instances
- ## Launch Template
	- AMI + Instance Type
	- EC2 User Data
	- EBS Volumes
	- Security Groups
	- SSH Key Pair
	- IAM Roles for your EC2 Instances
	- Network + Subnets Information
	- Load Balancer Information
- ## [[Practice]]
	- Create launch template
	- Set up capacity
	- Integrate with TG
	- Set up auto scaling:
		- Dynamic scaling based on CPU utilization
		- Check monitoring to see CPU increase (after stress CPU in one instance)
		- Check activity: ASG creates more instances