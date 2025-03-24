deck:: aws

- Steps
	- instance hibernates
	- RAM stopping
	- RAM state is preserved to EBS volume
	- RAM releases
	- instance start
	- RAM state from EBS is loaded
- Requirements #card #incremental
  id:: 67cdc2c4-fc3f-4a69-a432-97777b30af86
	- Root Volume **must be EBS**, **encrypted**, not instance store and large
	- **RAM size < 150GB if linux, < 16GB if windows**
	- Instance Size - not supported for bare metal instances
	- NOT hibernated more than **60 days**
	- Available for **On-Demand, Reserved and Spot Instances**
	-
	-