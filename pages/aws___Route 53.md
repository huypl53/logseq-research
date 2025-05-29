deck:: aws

- ## Steps
	- Create a domain
		- Add a record
			- Record name
			- Record type
			- Value: target ip address
	- test in on aws cloudshell
		- ```bash
		  yum install -y bind-utils
		  nslookup <created_dns>
		  
		  
		  dig <created_dns>
		  #	- return record type
		  ```
	- test on an EC2 instance
		- create **3 new EC2 instances in different AZ**
		- create an [[aws/EC2/ELB]] ALB
			- forward to a new [[aws/EC2/TG]]
				- attach a new created EC2
		- demo TTL:
			- add a new A record to domain
			- assign a public IP of created EC2 to record value
			- use `dig` command to resolve the TTL timeout
- ## Record
	- ?how to route traffic from a domain to a public IP
	- Each record contains:
		- Domain/subdomain Name – e.g., example.com
		- **Record Type** – e.g., A, AAAA, CNAME, NS, CAA, DS,...
			- A: IP v4
			- AAAA: IPv6
			- CNAME: map a hostname to another hostname
			- NS: name servers for the Hosted Zone
		- Value – e.g., 12.34.56.78 (IP address)
		- Routing Policy – how Route 53 responds to queries
		- TTL – amount of time the record cached at DNS Resolvers
- # TTL #card #incremental
  id:: 67e95184-c79d-4a02-9618-7dc27a8229e9
	- > time to live
	- client sends DNS request to Route 53
	- Route 53 returns the resolved IP + record type (e.g: A for IPv4) + TTL
	- client send request to the resolved IP address **util TTL timeout**, then send new request to Route 53 to **receive an updated ip**
	- **High TTL - e.g 24 hr**: less traffic on Route 53, possibly outdated records
	- **Low TTL -e.g 60 sec**: More traffic on Route 53 ($$), **easy to change records**
	- Except for alias records, **TTL is mandatory for each DNS record**