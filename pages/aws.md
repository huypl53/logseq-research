> tags:: [[cloud computing]]

-
- Horizontal Scaling
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
	- Create [[aws/EC2]] from launch template
	- Create [[aws/SG]] for the load balancer [[aws/EC2/LB]] to be able to listen on port 80
	- Create [[aws/EC2/TG]]. Then attach that [[aws/EC2/TG]] to the current [[aws/EC2/LB]], the load balancer will forward the requests to the [[aws/EC2/TG]]
	- Create [[aws/EC2/ASG]], attach it to [[aws/EC2/TG]]
	- {{renderer excalidraw, excalidraw-2025-02-20-21-24-15}}