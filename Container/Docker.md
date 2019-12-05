# Docker introduction
Docker is a computer program that performs operating-system-level virtualization, also known as 'containerization'.

# Installation
Follow the [official website](https://www.docker.com/get-started).

# Basic check
Check for Docker version: 
> docker --version 

Check for Docker basic status:
> docker info

# Manage Docker containers
## Create a container
Create a new container and run a command:
> docker run [OPTIONS] IMAGE [COMMAND]
Create a new container without running it:
> docker create [OPTIONS] IMAGE [COMMAND]

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/run/): 
* -d: Run as background task;
* -p: Port forwarding, format: 'HOSTPORT:CONTAINERPORT';
* -i: Interactive mode, keep STDIN open;
* -t: Allocate a terminal;
* --name="NAME": Assign a name to container;
* --link=[]: Link to another container

## Manage Docker containers
Manage containers with name or ID:
> docker start CONTAINER
> docker stop CONTAINER
> docker restart CONTAINER
> docker rm CONTAINER
> docker pause CONTAINER
> docker unpause CONTAINER

## Execute command in containers
Execute command in running Docker container:
> docker exec [OPTIONS] CONTAINER [COMMAND]

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/exec/): 
* -d: Run as background task;
* -i: Interactive mode, keep STDIN open;
* -t: Allocate a terminal;

## Listing
List out running containers:
> docker container ls [OPTIONS]

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/container_ls/): 
* -a: Show all;
* -q: IDs only;

List out images (hide intermediate images):
> docker image ls [OPTIONS]

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/image_ls/): 
* -a: Show all;
* -q: IDs only;

## Quick usage
Delete dangling images:
> docker rmi $(docker images -f "dangling=true" -q)
Show Docker size:
> docker system df
