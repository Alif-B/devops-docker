# Learning and Documenting Docker

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
