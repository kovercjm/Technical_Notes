# 0 Introduction
Docker is a computer program that performs operating-system-level virtualization, also known as 'containerization'.

# 1 Installation
Follow the [official website](https://docs.docker.com/engine/install/).

Check for Docker version: 

``` shell
docker --version 
```

Check for Docker basic status:

``` shell
docker info
```

Avoid using `sudo docker` every time, run the following command to add a docker user group.

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
* --name="NAME": Assign a name to container;
* --link=[]: Link to another container
* --restart: Restart policy to apply when a container exits
* --rm: Automatically remove the container when it exits

## 2.2 Manage Docker containers
Manage containers with name or ID:

``` shell
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER
docker pause CONTAINER
docker unpause CONTAINER
```

## 2.3 Execute command in containers

Execute command in running Docker container:

``` shell
docker exec [OPTIONS] CONTAINER [COMMAND]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/exec/): 
* -d: Run as background task;
* -i: Interactive mode, keep STDIN open;
* -t: Allocate a terminal;

## 2.4 Listing
List out running containers:

``` shell
docker container ls [OPTIONS]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/container_ls/): 
* -a: Show all;
* -q: IDs only;

List out images (hide intermediate images):

``` shell
docker image ls [OPTIONS]
```

OPTIONS [explainations](https://docs.docker.com/engine/reference/commandline/image_ls/): 
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

# 3 Installation without Internet

Following the [official guide](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package).

## 3.1 Preparation

Assume running under Ubuntu 18.04 OS, download three `deb` [image](https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/) (`containerd.io`, `docker-ce-cli` and `docker-ce`), and save to external disk for installation.

## 3.2 Install from deb image

May mount the disk and copy the images to local folder first.

Install Docker Engine with the following command, asuming the three images are all under `~/docker/`.

``` shell
sudo dpkg -i ~/docker/*.deb
```

## 3.3 Check installation

Using the same command as [Chapter 1](#1), to check installation and can avoid using `sudo`.