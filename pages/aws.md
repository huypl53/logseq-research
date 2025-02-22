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
	- Create [[aws/SG]] for the load balancer [[aws/EC2/LB]] to be able to listen on port 80
	- Create [[aws/EC2/TG]]. Then attach that [[aws/EC2/TG]] to the current [[aws/EC2/LB]], the load balancer will forward the requests to the [[aws/EC2/TG]]
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