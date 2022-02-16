# Docker Certified Associate Exam (DCA)

# Installing Docker

* Docker has 2 versions - Community Edition (CE), Enterprise Edition (EE)
* If you can run your application from a local or on premesis, CE is good enough
* To run docker in the cloud, you will definitely need the EE
* EE has Docker Control Plane (DCP) and Docker Trusted Registry (DTR)
* Sizing - 8GB RAM minimum for master and 4GB RAM min for worker nodes


## Using Docker

docker run -it <image_name>:<tag> <command>
Create a container with the latest version of Ubuntu image

Whenever you start a docker, it only run until in finishes the given command and then kills itself
so you can run it with -d command to run it in detached mode
This will keep the container running in the background even though it is not doing anything

use --rm option to automatically delete the container once the process is finished
use --name to give your conatiner a non-rng name. Docker network can use this as DNS
use --memory, --cpu-shares, --cpu-quota to control resource sharing between containers
use -p <host_port>:<container_port> option to expose port to outside of the container
use /<protocol> after the port to use a specific protocol
use --net to put the container in a specific network
use -e to set environment variables for the container
use --link to link 2 container
use -v to attach a volume to the container


docker start <container_name>
this starts an already created container in detached mode


docker attach <container_name>
to join the terminal of that container
after attaching if you quit then the container will stop
If you want it to keep running detached, the do `ctrl+p crtl+q` instead of exiting

docker exec <container_name> <command>
This will add add the command as a process to an already running container

docker logs <container_name>
This will show all the logs for the container, the logs are removed with the container

docker kill <container_name>
Stop the container but still keeps it around

docker rm <container_name>
Removes the container from Docker

docker port <container_name>
to find the exposed port of a container

docker network ls
shows all the networks available
Bridge Network - It's the default network provided by kubernetes. Use bridge whenever possible.
Host Network - It's the same network as your computer. (Bridge is isolated from your host network)

docker network create <network_name>
This creates a network of said name

docker network <network_name> <container_name>
This adds the container to the network
One container can have multiple networks

Docker Linking
This is a legacy version of network.
This only works one way (all the ports of A is mapped to all ports of B, but not the other way around)
It's best to avoid linking, but there are still some special use cases for it

docker commit <container_id> <new_image_name>:<tag>
This will create a snapshot of the container and create an image with it
docker tag <image_name> <tag>
This will tag the image

docker search <something>
To look up images from the terminal

docker pull / docker push
This downloads and uploads images to dockerhub registry
DO NOT push images with secrets in them to any registry

docker volume create <volume_name>
Persistant Volume - Data that stays even after the deletion of the container
Ephimeral Volume - Data that gets wiped when no container is using it
If the file you're sharing does not exist, docker shares it as a directory


## Building Docker Images

Dockerfiles are used to "program" images

docker build -t <tag> <directory_of_dockerfile>
this will build an image out of the file called `Dockerfile` in the current directory

The process is `Choose existing image` > `Make a container` > `Run stuff in it` > `Make it an image`

Dockerfile is IDEMPOTENT
This means that it won't rerun the steps that has already been run on reapplies
TIP: Put the part of the dockerfile that you change the most at the end of the file

Docker file is not shell script
Each line will run it's own container to run the command
So put all the things you want done together, in one line
However, environment variables will persist if you use ENV command to set them

Multistage Build
This is where you complie your application (IE an API), and take the binary
and start a smaller image with that binary in it.

For Example, you may need ubuntu to compile your api. This case in the dockerfile, we can
use an Ubuntu to image to compile the api. And since everyhting in the computer in binary anyway,
we take the binary of the API and inject it onto a smaller image like alpine and run it off of there.
Freeing up a lot of space in the process

Docker swarm
Alternative to kubernetes

Docker Swarm Locking
You can kind of put a pass code on your swarm. You will need that code to turn it on or restart it
You can autolock, change the passcode 

Service vs Container vs Task vs Stack
Container - Is one instance of the image
Service - Is a set of containers that run an application (IE node1, node2)
Stack - Is a set of services that make up the application (IE webserver, DB server etc)
Task - Is Container + Set of commands to run in the container

Quorum
Basically a consensus mechanism among multiple master nodes.
(N/2)+1 of the master nodes must be available at all time for you to manage the cluster

Replicated VS Global Services
Replicated - Identical services are distributed Throughout all node (IE nodes have 3 webservers and 5 db servers etc)
Global - Only one service per node (IE each node has only one DataDog Agent service)

Block Storage VS Object Storage
Block Storage - Writes a chunk of data on some area of the storage, therefore has no metadata. Hence best for high IOPS
Object Storage - Writes with meta data and a unique identifier, therefore it's more scaleable. But there is no hierarchy

Storage Driver
It's basically the program that controls writeable layers of containers
Based on your OS Docker recomends drivers that you should be using
Once you change the driver, any container or images that you created using the previous driver will be lost
For that, you can save and push all the container and reload them once the driver has been changed

Storing Persistant Data
Volumes (Docker Managed) > Bind Mount (Host Managed) > tmpfs (On RAM)

Volume Drivers / Plugins
Docker has many plugins for you to read and write data to and from different storage providers like Azure, GCP, AWS, VMWare etc