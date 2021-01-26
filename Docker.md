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

<p>
libnetwork implements Container Network Model (CNM), which is the specification for docker networking. I uses a system of drivers that extends them model by network topologies.
</p>

### Network Drivers:

* Bridge is a Link layer device that forwards traffic between network segments. It allows containers connected to the same bridge network to communicate. Also isolates these containers from the other containers that are not connected to the same network. It is the default that is used everytime that we create a new container. This driver only works on Linux.

* Everytime that we want to create a distributed network among multiple docker hosts, we use the overlay network driver. A good example is when using Swarm.

* "macvlan" network driver allows us to assign a Mac address to a container and treat it as a real physical device on the network.

* "non" driver is used to disable networking for a container. It is mostly used with a custom networking driver.

* There are also third party "Network plugins" available with docker.

### Closer Look at CNM:

It consists of three building blocks:

- Sandboxes: Isolates the network stack, which includes: Networking Interfaces, Ports, Rout tables and DNS.
- Endpoints: Virtual Network Interfaces, It connects the sandbox to a network. Since they act like real network adaptors, they can only be connected to a single network.
- Networks: software implementations of 802.1d bridge.

### Docker Networking Commands:

To list all the networks created on the host:

    docker network ls

To get detailed info on a network:

    docker network inspect <name>

To create a network:

    docker network create <name>

To remove a network:

    docker network rm <name>

To remove all unused networks:

    docker network prune

To add a container to a network:

    docker network connect <network> <container>

To create a container that uses a network:

    docker container run -name <name> -it --network <network> <image> /bin/bash

To remove a container from a network:

    docker network disconnect <network> <container>

To create a network with a subnet and gateway:

    docker network create --subnet <subnet> --gateway <gateway> <name>

For example:

    docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 br02

and:

    docker network create --subnet 10.1.0.0/16 --gateway 10.1.0.1 \
    --ip-range=10.1.4.0/24 --driver=bridge --label=host4network br04

Assigning an IP to a container:

    docker container run -name <name> -it --network <network> --ip <IP> <image> <CMD>

Adding a container to a network:

    docker network connect <network> <container>

## Docker Storage

