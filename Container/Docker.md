# 0 Introduction
Docker is a computer program that performs operating-system-level virtualization, also known as 'containerization'.

# 1 Installation
Follow the [official website](https://docs.docker.com/engine/install/).

Check for Docker version and basic status: 

``` shell
docker --version
docker info
```

To avoid using `sudo docker` every time, run the following command to add a docker user group.

``` shell
# sudo groupadd docker							# Add docker group if not exist
sudo usermod -aG docker your-user		# Add your-user to docker group
newgrp docker												# Active change to docker group
```

# 2 Manage Docker containers

## 2.1 Create a container
Create a new container and run a command:

``` shell
docker run [OPTIONS] IMAGE [COMMAND]
```

Create a new container without running it:

``` shell
docker create [OPTIONS] IMAGE [COMMAND]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/run/): 
* -d: Run as background task;
* -p: Port forwarding, format: 'HOSTPORT:CONTAINERPORT';
* -i: Interactive mode, keep STDIN open;
* -t: Allocate a terminal;
* --name NAME: Assign a name to container;
* --restart: Restart policy to apply when a container exits;
* --rm: Automatically remove the container when it exits;

## 2.2 Manage Docker containers
Manage containers with name or ID:

``` shell
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER
```

## 2.3 Execute command in containers

Execute command in running Docker container:

``` shell
docker exec [OPTIONS] CONTAINER [COMMAND]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/exec/), similar to [Create container](#2.1).
## 2.4 Listing
List out running containers or images:

``` shell
docker container ls [OPTIONS]
docker images [OPTIONS]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/container_ls/): 
* -a: Show all;
* -q: IDs only;

## 2.5 Quick usage
Delete dangling images:

``` shell
docker rmi $(docker images -f "dangling=true" -q)
```

Show Docker size:

``` shell
docker system df
```

Cleaning not used volume:

``` shell
docker volume rm $(docker volume ls -qf dangling=true)
```



## 2.6 Save Docker image

Save Docker image and load at another machine.

``` shell
docker save -o images.tar image1 image2
docker load -i images.tar
```

# 3 Compile an Docker image

