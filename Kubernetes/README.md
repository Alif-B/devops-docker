# Kubernetes

# Introduction
* Container Orchastration Tool
* Was originally designed by Google to manage it's 2 Billion container deployments per week (BORG)
* BORG was modified and improved to be Kubernetes now

### Features
* High Availability - 
* High Scalability - 
* Disaster Recovery -

### Architecture
* One master node called Control Plane and multiple worker nodes
* Master node has 3 things
    * API Server - connects to the clients (IE Kube Dashboard, Kubeclt, API Scripts)
    * Controller Manager - Makes sure everything is working as it should. Nothing is breaking down
    * Scheduler - Assigns nodes to pods
    * etcd database - Containes the states and configuration of the cluster (not data)
* Multiple master node is basically a necessary
* Kubectl has a file called kubeconfig where all the server info and authentication info are stored
* kubelet is a process run on worker nodes that reports back to the API
* kube-proxy is the process that handles network routing and load balancing
* So a end user requests for the cluster will come into the kube-proxy and get routed


* Node - A physical or virtual server
* Pod - The smallest managable unit in kubernetes. It container multiple container
    * Each pod has it's own IP address

* A node must have 4 things - kubelet | kube-proxy | docker (or alt) | supervisord

# Controllers
* Individual Pods are not self-healing. Controllers can take care of that
* Controllers provide - Scalability, Availability and Load Balancing

### Types of Controllers
* ReplicaSets - The number of pods fixed, not available for individual pods. Needs deployment for it
* Deployments - Basically when you specify the end state of the cluster through a file.
* Jobs - Are like cronjobs. Repeat at a certian times. Used for regular backups and stuff
* DaemonSets - Its like the Global Service in Docker Swamr. Used form loggers and monitors
* Services - Services allow communication between deployments and pods.

* Deployments can be paused. That means all the updated are paused, the traffics are still routed to the right pods
* Services get one unique IP address throughout it's lifetime
* Podspec is a YAML file that describes a pod. Keubelet uses this to run the pod accordingly
* Service is beasically network. There are internal and external services. Along with load balancing services for cloud providers

```kubectl create -f <deploymentfile>``` - Will create the deployment mentioned in the file
```kubectl expose deployment <deployment-name> --port=NodePort``` - Exposes the deployment as a service
```kubectl get deployment/pod/service/node``` - Shows all the components
```kubectl get deployment/<deployment-name> -o yaml``` - shows all the details for the deployment in yaml format