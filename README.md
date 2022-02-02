# Learning and Documenting Docker

## Using Docker

docker run -it <image_name>:<tag> <command>
Create a container with the latest version of Ubuntu image

Whenever you start a docker, it does something and kills itself
so you can run it with -d command to run it in detached mode
This will keep the container running in the background even though it is not doing anything

use --rm option to automatically delete the container once the process is finished
use --name to give your conatiner a non-rng name


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

