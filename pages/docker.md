## Swarm
collapsed:: true
	- Nodes
		- `Manager node`: distributes tasks to `worker nodes`
	- Swarm
		- contains list of nodes which are installed in machines, virtual machines, AWS EC2,...
		- communicate via docker overlay network
-
- ## Today
  collapsed:: true
	- pid:
	-
- ## Experience
	- ### Cuda
		- **Error response from daemon: could not select device driver "nvidia" with capabilities: \[[gpu]]**
			- Troubleshoots: 'NVIDIA Container Toolkit' was not installed
			  id:: 69532c75-1d5d-42a9-b0f0-3b8fbd0054cd
			- Solution:
			  collapsed:: true
				- ```
				  curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey |sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
				  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
				  && sudo apt-get update
				  
				  apt-get install -y nvidia-container-toolkit
				  
				  nvidia-ctk runtime configure --runtime=docker
				  
				  systemctl restart docker
				  ```