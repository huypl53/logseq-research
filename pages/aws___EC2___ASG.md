- > Auto Scaling Group
- be able to create new instance from provided Launch template
- add/remove EC2 in [[aws/EC2/TG]] obeyed the defined capacity (desired, min, max)
- like other nodes in AWS, [[aws/ec2/asg]] need to attach to a [[aws/SG]] for publishing port 80
- manual config
- dynamic config
	- use [[aws/CloudWatch/Alarms]], which trigger action in [[aws/EC2/ASG]] to add/remove unit capacity. Check result in history