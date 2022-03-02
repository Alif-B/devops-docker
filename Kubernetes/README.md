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

* Node - A physical or virtual server
* Pod - The smallest managable unit in kubernetes. It container multiple container
    * Each pod has it's own IP address

