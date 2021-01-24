# Docker
To install docker on linux:

    wget -qO- https://get.docker.com |sh

      sudo systemctl start docker

      sudo systemctl enable docker

## Running a Docker Image
To run a Docker:

    docker run <container_name>

To list running containers:

    docker container ls

To list all containers:

    docker container ls -a

To run a docker in interactive mode and assign a name to it:

    docker run -it --name <nickname> <imagename>

To run a persistent container that doesn't shut off on exit and restarts:

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

## Managing Containers

To remove a container:

    docker rm <nickname>

To remove all stopped containers:

    docker container prune

To rename a container:

    docker rename <nickname> <newnickname>

To get metrics about all containers:

    docker stats

To get metrics about a specific container:

    docker stats <nickname>

## Manging Images:

To create a container image:

    docker commit <nickname> <imagename>

To pull an image:

    docker pull alpine

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

Expose a port:

    Expose <port>

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