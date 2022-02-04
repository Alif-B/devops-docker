# This is sample docker file that is helping me learn dockerfiles
# Official Docs - https://docs.docker.com/engine/reference/builder/

FROM ubuntu:latest as builder

# Basic tagging for the image
LABEL name="Sheikh Billah (Alif)"
LABEL email="ambam3110@gmail.com"

# Adds the environment variable to every container through out this build
ENV secrets="my facebook password"

# Every command in this container will be run as the specified user
USER root

# Every command beyond this point will run in the specified directory
# As well as make it the starting directory of this image
WORKDIR /

# Run some shell scripts in the container
RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get -y install vim

# Makes port 8080 available to all the other container
EXPOSE 8080

# Adds localfiles, downlaods remote file, unarchived tar.gz file
# If you want to downlaod or unarchive,
# Make sure to add a directory and not a file
ADD secret.txt /secret.txt
ADD https://github.com/phayes/geoPHP/tarball/master /tarfile/

# Attaches volumes - 1 arg means ephimeral | 2 args mean persistant
VOLUME ["/ephimeral_volume"]
VOLUME ["/.vscode", "/vscode_data"]

# This is like CMD, but any extra argument after docker run will get added after ls
ENTRYPOINT ls

# This is the command that will run on that image startup
# Any extra args after docer run will replace CMD
# If you have both ENTRYPOINT and CMD they get strung together in order
CMD ["/bin/vim", "/secret.txt"]


# Multi-stage build
FROM alpine
COPY --from=builder /secret.txt /secret.txt
ENTRYPOINT echo The secrets in the file is; cat /secret.txt