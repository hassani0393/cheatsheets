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

## Docker Storage Concepts

### Categories of data storage:
* Non-persistent: short-lived data that is tied to the lifecycle of the container like application data. Every container has it.
* Persistent: We want it to stick around. We used volumes to do so. Volumes live outside of containers. Like data in a database that the application uses.

<p> Non-persistent storage is also called local storage and snapshot storage. It's usually found in /var/lib/docker/"STORAGE-DRIVER". </P>

### Storage Drivers:
* RHEL: overlay2
* Ubuntu: overlay2 or aufs
* SUSE: btrfs
* Windows: uses it's own default storage driver.

### Ways to handle persistent data:
* Use a volume for persistent data
* bind mounts

### To use volumes:
1. Create the volume.
2. Create the container.
3. Mount the volume to a directory in the container.

<p> Deleting the container does not delete the volume. Volumes are first-class citizens, which means they are their own object,they have their own APIs as well as their own sub-commands. The local driver is used by default. This means that when we create a volume, it's created on the local driver on the docker server.</p>

* Volumes are created by default on /var/lib/docker/volumes/

### Docker Volume Commands

To list all docker volume commands:

    docker volume -h

To list all volumes on a host:

    docker volume ls

To create a volume:

    docker volume create <name> # To set the driver use -d. Use --label to set metadata. Use -o to set driver specific options.

To inspect a volume:

    docker volume inspect <name> // If a volume name is not given it's going to set a volume ID.

To delete a volume:

    docker volume rm <name>

To delete all unused volumes:

    docker volume prune

To use the mount flag for mounting a volume on a container:

    docker container run -d --name <Name> --mount type=volume,source=<thevolume>,target=<mountLocationInTheContainer> <nameofImage>

To create a volume using volume flag:

    docker container run -d --name <NAME> -v <Volume-name>:<target> <image>

To create a volume that is read-only by the container:

    docker run -d --name=web01 --mount source=html-volume,target=/usr/share/nginx/html,readonly nginx


### Using Bind Mounts

Using the mount flag:

    docker container run -d --name <name> --mount type=bind,source=<SOURCE>,target=<TARGET> <IMAGE>

Using the volume flag:

    docker container run -d --name <NAME> -v <SOURCE>:<TARGET> <IMAGE>

## Docker Instructions

- From: Initializes a new build stage and sets the base 
image.

- RUN: Executes any commands in a new layer.

- CMD: Provides a default for an executing container. There can only be one CMD instruction in a dockerfile.

- LABEL: Adds metadata to an image.

- EXPOSE: Informs Docker that the container listens on 
the specific network ports at runtime.

- ENV: Sets the environment variable "key" to the "value"

- COPY: Copies new files or directories from "src" and adds them to the filesystem of the container at the path 
"dest".
- ADD: Copies new files, directories or remote file URLs 
from "src" and adds them to the filesystem of the image 
at the path "dest".

- ENTRYPOINT: Allows configuring a container that will run as an executable.

- VOLUME: Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other container.

- USER: Sets the username and optionally the user group to use when running the image and for any RUN, CMD, and ENTRYPOINT instructions that follow it in the dockerfile.

    RUN useradd -ms /bin/bash cloud_user
    USER cloud_user

- WORKDIR: Sets the working dir for any RUN, CMD, ENTRYPINT, COPY and ADD instructions that follow it in the dockerfile.

- ARG: Defines a variable that users can pass at build-time to the builder with the docker build command, using the --build-arg "varname"="value" flage.

- ONBUILD: Adds a trigger instruction to the image that will be executed at a later time, when the image is used as the base for another build.

- HEALTHCHECK: Tells Docker how to test a container to check that it is still working.

- SHELL: Allows the default shell used for the shell form of commands to be overriden.

### Environment Variables

When building an image, we can use "--env" flag to pass an environment variable:

    --env [KEY]=[VALUE]

To use the ENV instruction in dockerfile:

    ENV [KEY]=[VALUE]
    ENV [KEY] [VALUE]


### Build Arguments

Using build argument flag in the image building:

    --build-arg [NAME]=[VALUE]

Using the ARG instruction in the Dockerfile:

    ARG [NAME]=[DEFAULT_VALUE]



## Docker Compose

### Downloading and Installing Docker Compose

Downloading the latest version of Docker Compose:

    sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

Apply executable permissions:

    sudo chmod +x /usr/local/bin/docker-compose

Test Docker Compose:

    docker-compose --version

### Compose Commands:

Create a docker compose file


* docker-compose.yml
    version: '3'
    services:
       web:
         image: nginx
         ports:
         - "8080:80"
         volumes:
         - nginx_html:/usr/share/nginx/html/
         links:
         - redis
       redis:
         image: redis
    volumes:
       nginx_html: {}

Create a compose service and start containers(-d: run containers in the background):
    
    docker-compose up -d: 

List the containers created by compose:

    docker-compose ps

Stop a compose service:

    docker-compose stop

Start a compose service:

    docker-compose start

Restart a compose service:

    docker-compose restart

Delete a compose service:

    docker-compose down

### Management Commands:

- up: Create and start containers

- ps: List containers

- stop: Stop services

- start: Start services

- restart: Restart services

- down: Stop and remove containers, networks images, and volumes

-test
