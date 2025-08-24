## Components
	- API server:
		- helps components communicate and co-work
	- Node
		- Master node: the server takes control over all activities
			- Scheduler:
			- Controller manager:
				- worker activity management
			- Etcd:
				- Database
		- a node can **have multiple pods**
		- run at least a `kubelet`
		- run at least a container runtime (like docker) responsible for pulling the container image for ma registry, unpacking container and running the application
	- Pod
		- Is a **group of containers** deployed within a node
		- Pod containers **share IP address, IPC, hostname**
		- Pod detaches **network and storage from containers** -> easy to move containers around cluster
		- By default, the Pod is only accessible by its **internal IP address** within the cluster -> to make **container accessible from outside** k8s virtual network, you have to expose the Pod as a `service`
		- **models an application-specific** *logical host* and can contain coupled containers: a nodejs server and a data container that feeds data to be published by the nodejs server.
	- Replication controller
		- manage pod replication
	- Service
		- route task to pod
		- types:
			- *ClusterIP* (default) - Exposes the Service on a**n internal IP in the cluster**. This type makes the Service only reachable from within the cluster.
			- *NodePort* - Exposes **the Service on the same port of each selected Node** in the cluster using NAT. Makes a Service accessible from outside the cluster using `NodeIP:NodePort`. Superset of ClusterIP.
			- *LoadBalancer* - Creates an external load balancer in the current cloud (if supported)
			  and **assigns a fixed, external IP to the Service**. Superset of NodePort.
			- *ExternalName* - Maps the Service to the contents of the `externalName` field
			  (e.g. `foo.bar.example.com`), by returning a `CNAME` record with its value.
			  No proxying of any kind is set up. This type requires v1.7 or higher of `kube-dns`,
			  or CoreDNS version 0.0.8 or higher.
		- e.g:
			- expose service outside of cluster
				- ```
				  kubectl expose deployment hello-node --type=LoadBalancer --port=8080
				  ```
				- The `--type=LoadBalancer` flag indicates that you want to expose your Service
				  outside of the cluster.
		- A Service created without `selector` will also not create the corresponding Endpoints object. This allows users to manually map a Service to specific endpoints.
			- Another possibility why there may be no selector is you are strictly
			  using `type: ExternalName`
	- Kubelet
		- run on nodes, make sure container start and work
		- responsible for communication **between the control plane and the Node**
	- Kubectl
		- > kubectl <action> <resource>
		- *action*: create, describe, get
		- *resource*: node, deployment
	- Image
	- Control plane
		- > coordinates the cluster
		- control plane automatically **handles scheduling the pods across the Nodes** in the cluster.
	- Deployment
		- instructs k8s how to create and update instances
		- checks on the health of your Pod and **restarts the Pod's Container if it terminates**.
		- when being created, create a new Pod
- ## Practices
	- minikube
		- local k8s focusing in learning
	- create a deployment
		- ```
		  kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
		  ```
	- use `kubectl proxy` to temporarily expose pod publicly
		- or create a `LoadBalancer` service for that deployment
	- start bash session inside the pod's container:
		- ```
		  export POD_NAME="$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')"
		  kubectl exec -ti $POD_NAME -- bash
		  ```
	- Scale apps
		- First, create a service typed `LoadBalancer`
		- ```
		  kubectl scale deployments/kubernetes-bootcamp --replicas=4
		  ```
- ## Keywords
	- affinity
	- anti-affinity
	- rolling updates container: after isolating container
	- SLA