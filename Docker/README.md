# Docker Certified Associate Exam (DCA)

# Installing Docker

* Docker has 2 versions - Community Edition (CE), Enterprise Edition (EE)
* If you can run your application from a local or on premesis, CE is good enough
* To run docker in the cloud, you will definitely need the EE
* EE has Docker Control Plane (DCP) and Docker Trusted Registry (DTR)
* Sizing - 8GB RAM minimum for master and 4GB RAM min for worker nodes
* Best to troubleshoot UCP Clusters with client bundles


# Using Docker

```docker run -it <image_name>:<tag> <command>```  
Create a container with the latest version of Ubuntu image

Whenever you start a container, it only run until in finishes the given command and then kills itself  
so you can run it with -d command to run it in detached mode  
This will keep the container running in the background even though it is not doing anything.    

* use --rm option to automatically delete the container once the process is finished  
* use --name to give your conatiner a non-rng name. Docker network can use this as DNS  
* use --memory, --cpu-shares, --cpu-quota to control resource sharing between containers  
* use -p <host_port>:<container_port> option to expose port to outside of the container  
* use /<protocol> after the port to use a specific protocol (IE 8080:8080/TCP)  
* use --net to put the container in a specific network  
* use -e to set environment variables for the container  
* use --link to link 2 container  
* use -v to attach a volume to the container  
* use -m to limit the max memory a container can use  
* use --privileged to enable kernal level capabilities  
* use --pretty to print it in an easily readable format


# Building Docker Images

Dockerfiles are used to "program" images
The process is `Choose existing image` > `Make a container` > `Run stuff in it` > `Make it an image`

### Dockerfile is IDEMPOTENT
This means that it won't rerun the steps that has already been run on reapplies   
TIP: Put the part of the dockerfile that you change the most at the end of the file   

### Docker file is not shell script
Each line will run it's own container to run the command   
So put all the things you want done together, in one line   
However, environment variables will persist if you use ENV command to set them   

### Multistage Build
This is where you complie your application (IE an API), and take the binary   
and start a smaller image with that binary in it.   

For Example, you may need ubuntu to compile your api. This case in the dockerfile, we can   
use an Ubuntu to image to compile the api. And since everyhting in the computer in binary anyway,   
we take the binary of the API and inject it onto a smaller image like alpine and run it off of there.   
Freeing up a lot of space in the process   

# Docker swarm
Alternative to kubernetes   

### Docker Swarm Locking
You can kind of put a pass code on your swarm. You will need that code to turn it on or restart it   
You can autolock, change the passcode    

### Service vs Container vs Task vs Stack
* Container - Is one instance of the image  
* Service - Is a set of containers that run an application (IE node1, node2)  
* Stack - Is a set of services that make up the application (IE webserver, DB server etc)  
* Swarm - a collection of nodes. You run all of your stacks in the swarm  
* Task - Is Container + Set of commands to run in the container  

### Quorum
Basically a consensus mechanism among multiple master nodes.   
(N/2)+1 of the master nodes must be available at all time for you to manage the cluster
  
### Replicated VS Global Services
Replicated - Identical services are distributed Throughout all node (IE nodes have 3 webservers and 5 db servers etc)   
Global - Only one service per node (IE each node has only one DataDog Agent service)  

# Docker Storage
  
Persistant Volume - Data that stays even after the deletion of the container  
Ephimeral Volume - Data that gets wiped when no container is using it   

### Block Storage VS Object Storage
Block Storage - Writes a chunk of data on some area of the storage, therefore has no metadata. Hence best for high IOPS   
Object Storage - Writes with meta data and a unique identifier, therefore it's more scaleable. But there is no hierarchy   

### Storage Driver
It's basically the program that controls writeable layers of containers   
Based on your OS, Docker recommends drivers that you should be using   
Once you change the driver, any container or images that you created using the previous driver will be lost   
For that, you can save and push all the container and reload them once the driver has been changed   

### Storage Scalability Preference
Volumes (Docker Managed) > Bind Mount (Host Managed) > tmpfs (On RAM)

### Volume Drivers / Plugins
Docker has many plugins for you to read and write data to and from different storage providers like Azure, GCP, AWS, VMWare etc
  
# Docker Networking
### Contianer Network Drivers
* Bridge Network - Default Driver. It bridges the host and the container with port mapping  
* Host Network - It's directly on the host's network. AKA container and the host can't use the same ports  
* None - When you want the container to be completely isolated from any networking   
### Service Network Drivers
* Overlay Network - Only available for EE and Swarm. It connects multiple hosts and their containers together  
* Ingress Network - A type of overlay network that load balances among the service nodes  
* docker_gwbridge - It connects the swarms virtual networks to the docker daemon's physical network  
* Macvlan Network - Containers get a MAC address therefore, they appear to be physical hosts to the network  

* Third party drivers - As it sounds, you can get custom network plugins
 
### Docker Linking
This is a legacy version of network.  
This only works one way (all the ports of A is mapped to all ports of B, but not the other way around)  
It's best to avoid linking, but there are still some special use cases for it  

# Docker Security

### Container Security
* Kernal Namespace - A container has it's own namespace at the kernal level, aka it's capabilities are isolated  
* Control Groups - Basically limitation on how much resources a container can have  
* Daemon Attack Surface - Is small, but a node should ONLY run docker and no other applications  
* Linux Kernal Capabilities - Even though containers run on the host as sudo, they have limited privileges on the host  
  
### Swarm Security
* MTLS - Mutually Authenticated TLS, every 3 months swarm automatically changes certificates without downtime  
* Manager nodes act as the certificate authority

  
# Some fun facts
* Docker Compose is a yaml file (defines services) used for orchastration and Dockerfiles are used to create images   
* Docker compose can build images for the service if a dockerfile is specified, but can't create an image   
* If you run docker compose on a swarm, it will ONLY run it on one worker node   
* Docker Compose V3 or higher is required to use compose with docker stack   
* During backups FROM MANAGER nodes. backing up only 1 node is enough, since all managers store the same data   
* Docker secrets is available only if you are running services   
* If you have both `ENTRYPOINT` and `CMD` in Dockerfile, they will get appended `[Entrypoint + CMD]` on execution   
* `.dockerignore` file can be used to ignore files in the directory   
* Can use `--no-cache` or `--no-cache=true` to disable caching while doing `dokcer build`   
* Default log driver format is json   
* Docker uses `iptables rules + namespaces` to isolate the containers on linux   
* By default containers can only communicate using the ip and NOT the host name   
* `--link` allows you to comm with another container without exposing ports. Will soon be depricated   
* You need to be in the manager node to be able to create overlay networks   
* Defult IP pool used by docker for overlay network is `10.0.0.1/8`   
* You can use `seccomp = secure computing` and it's filters to restrict access to your applications   
* `SELinux` is a set of kernal mods that allow you to have more control over system access   
* Any process run by Docker is run as `root` on the host, but you can use `--user` flag to change that   
* If lost the join-token, need to change it manually in both the managers and the workers   
* Docker secrets are immutable and they use `ramdisk` to attach as volume   
* `bind mounts` are read only so container does not mess up host files   
* `tempfs mount` avoids writing to the container's writeable layer (better container performance)(only availble in linux)   
* `loop-lvm` is for testing and `direct-lvm` is used for production environment   
  
  
* Secrets are not backed up when backing up UCP   
* Back up policy order - `Swarm > UCP > DTR`   
* To distribute managers accross mutiple nodes, you need to look at node failure situation and then quorum + fault taularence   
* Users, Orgs and Teams are not backed up while backing up DTR   
* Every node that will run DTR, requires UCP to be installed   
* `ADD` in Docker file is the same as `COPY` except, it can also copy from a url as well as copy tar files and extract it   
* You can use S3 as a storage backend for DTR, CAN NOT use EBS Volumes for that   
* You can only run Docker stack commands on the manager node   
* Configs inside the container is stored at `/` and secrets are stored at `/run/secrets`   
* Docker Swarm uses ports `TCP/2377 - Cluster Management Comm` and `TCP,UPD/7946 - Comm among nodes`   
* Any work after the docker run command will replace the CMD in the dockerfile   
* You need to create a custom network first, before allocating IPs to contianers   
* Docker swarn by default creates `docker_gwbridge- to provision overlay` & `ingress- to publish ports` on init   
* If you lose the root key in DCT, it is recommended to call Docker Support   
* Client Bundle is a set of certs that can be downloaded from UCP, It allows remote docker clients to log in as an EE user   
* One container can have multiple networks   
* DO NOT push images with secrets in them to any registry   
* If the file you're sharing in the Dockerfile does not exist, docker shares it as a new directory   
  
* ARG is the only instruction that can come before FROM in the Dockerfile. It's an argument can be provided with docker build
* EXPOSE command in Docker file doesn't actually publish the port, it's just a doc. You need to maually use -p while starting  
* Multiple WORKDIR gets appended and not replaced  
* ONBUILD in dockerfile is an instruction that will get triggered when the imgae is used for multi stage builds  

# Some Docker Commands
```docker swarm join-token worker``` - if you lost the command to make worker nodes   
```docker swarm unlock``` - To unclock the swarm  
```docker swarm update --auto-lock=true/false``` - To lock/ unlock the existing swarm  
```docker swarm unlock-key``` - Get the unlock key if you are logged in already   
```docker swarm unlock-key --rotate``` - To rotate the keys  
```docker service scale <service>=5``` | ```docker service update --replicas=5 <service>``` - scale to 5 replicas   
```docker service ps <service>``` - Lists the tasks inside the service  
```docker service rollback <service>``` - To revert the updates made to a service  
```docker service create --mount nginx``` - to add a volume to the service, does NOT support `-v`  
```docker stack services``` - to see the services in the stack   
```docker node update --lable-add / --lable-rm``` - to add or remove labels  
```docker network create --driver <network_type> <name>``` - To create a network  
```docker build <git_url>``` - You can just do that to run straight from github   
```docker swarm init --default-addr-pool 172.16.2.0/24``` - to specify address pool   
```docker run --net <nettwork> --ip <IP> ubuntu``` - Allocating static IP to a container   
```docker start <container_name>``` - this starts an already created container in detached mode   
```docker attach <container_name>``` - to attach you terminal to a detached container. `ctrl+p crtl+q` to exit, but keep detached   
```docker exec <container_name> <command>``` - This will add the command as a process to an already running container   
```docker logs <container_name>``` - This will show all the logs for the container, the logs are removed with the container   
```docker kill <container_name>``` - Stop the container but still keeps it around   
```docker rm <container_name>``` - Removes the container from Docker   
```docker port <container_name>``` - to find the exposed port of a container   
```docker network ls``` - shows all the networks available   
```docker network create <network_name>``` - This creates a network of said name   
```docker network <network_name> <container_name>``` - This adds the container to the network   
```docker commit <container_id> <new_image_name>:<tag>``` - This will create an image of a container   
```docker tag <image_name> <tag>``` - This will tag the image   
```docker search <something>``` - To look up images in DTR from the terminal   
```docker pull / docker push``` - This downloads and uploads images to dockerhub registry   
```docker volume create <volume_name>``` - Creates a volume   
```docker build -t <tag> <directory_of_dockerfile>``` - builds an image out of the `Dockerfile` in the current directory   
```sudo kill -SIGHUP $(<docker preocess id>)``` - reloads docker configs without killing the daemon   


