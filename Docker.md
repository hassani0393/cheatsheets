# Docker CLI

To see a list of the Docker commands:

    docker -h

To list commands for a specific topic:

    docker image -h

To install docker on linux:

    wget -qO- https://get.docker.com |sh

      sudo systemctl start docker

      sudo systemctl enable docker


## Creating Containers
To run a container from an image:

    docker run <imagename>

To run a command in a new container:

    docker container run <imagename>

To have the container be deleted after it's run:

    docker container run --rm <imagename>



To list running containers:

    docker container ls

To list all containers:

    docker container ls -a

To run a docker in interactive mode and assign a name to it:

    docker run -it --name <nickname> <imagename>

To run a persistent background container that doesn't shut off on exit and restarts in case of shutdown:

    docker run -dt --name <nickname> --restart <restartArg> <imagename> #always, unless-stopped, on-failure, no

To have it remove the container upon the exit:

    docker run -it --name rm-test --rm alpine



## Starting and Stopping an Existing Container

To start a container:

    docker container start <nickname>

To issue a command to a container:

    docker exec <nickname> sudo yum install ...

To drop into the containers shell:

    docker exec -it <nickname> <shellname>

To copy file from or to a container:

    docker cp <nickname>:/etc/location /location/

To stop a container:

    docker stop <nickname>

To restart a container:

    docker restart <nickname>

To pause all processes within one or more containers:

    docker container pause <id>

To unpause all processes within one or more containers:

    docker container unpause <id>


## Managing Containers

To remove a container:

    docker rm -f <id/nickname>

To remove all stopped containers:

    docker container prune

To rename a container:

    docker rename <nickname> <newnickname>

To get metrics about all containers:

    docker stats

To get resource usage metrics about a specific container:

    docker stats <nickname>

To get a listing of all running containers:

    docker container ps

To get a listing of info about a container:

    docker container inspect <id>

To display running process of a container:

    docker container top <id>

To get log data of a container:

    docker container logs <id>

To drop in prompt in a running container:

    docker container exec -it <id> /bin/bash

To run a command in a running container:

    docker container exec -it <id> <command>

To export a container's filesystem as a tar archive:

    export


## Manging Images:

To create a container image:

    docker commit <nickname> <imagename>

To pull an image:

    docker image pull alpine

To view pulled images:

    docker image ls

To remove an image:

    docker image rm <imagename>

To remove unused images:

    docker image prune -a

To get info about an image:

    docker image inspect <imgname>



## Dockerfiles

To build a docker from a dockerfile with a name:

    docker build <filedir> -t <nickname>

To run a container:

    docker run -dt --name app01 appimage

To provide a parent/scratch image:

    FROM <image|scratch>

To run a command against the default shell:

    RUN <command>

To set the working directory:

    WORKDIR <directory/path>

To copy files locally:

    COPY --chown <user> <source> <destination>

To copy files (could be from URLs):

    ADD <source> <destination>

To switch to a user:

    USER <user>


To set commands to run the container starts:

    CMD [ "<executable>", "<param1>", "<param2>" ]

To map container port to host server:

    docker run -p <host_port>:<container_port> <nickname>

## Using Docker Hub

To log in it the Docker Hub:

    docker login --username=<username>

Tag an image for a repo:

    docker tag <image> <username>/<repo>:<version>

Push to repo:

    docker push <username>/<repo>

## Exposing and Publishing Container Ports

To expose a port we use --expose [PORT]. For example:

    docker container run --expose [portnum] [image]

To publish a port(Map a container's port to a host's port):

    docker container run -p [HOST_Port]:[CONTAINER_Port] [IMAGE]



    docker container run -p [HOST_PORT]:[CONTAINER_PORT]/tcp -p [HOST_PORT]:[CONTAINER_PORT]/udp [IMAGE]

To publish all exposed ports to random ports

    docker container run -P

To list all port mappings or a specific mapping for a container:

    docker container port [containername]

## Docker Logging

Show information logged by a running container:

    docker container logs <containername/id>

Show information logged by all containers participating in a service:

    docker service logs <service>

<p> Logs need to be output to STDOUT and STDERR. </p>


## Docker Networking:

Open-source pluggable architecture
Container Network Model (CNM)
libnetwork implements CNM
Drivers extend the network topologies


