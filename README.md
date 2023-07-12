# Docker-Notes
https://www.youtube.com/watch?v=pg19Z8LL06w&t=5s
## Introduction
### What is a docker?
Docker is a container that encapsulates all programs developed along with its dependencies. This ensures that the program developed would still run correctly on a deployment server or machine. 

### Docker on a different OS
Program developed on a Docker cannot be transverse to another OS platform it is not installed one.
- e.g. A program developed in a docker that is installed on windows cannot be imported into a docker installed on linux.
- A docker is dependent on the OS it is installed on.

### Dockerhub
Dockerhub contains all Docker images built by the community or the organisation. For instance, you could find postgresql and redis images on [DockerHub](https://hub.docker.com). - You do not need to register to browse docker images.
***
## Installation
Install [docker](https://www.docker.com/products/docker-desktop/) for your OS and you will be good to go.
***

## Basic docker commands
The following functionalities will be done in the command prompt.
### 1. Check for Docker images
```
docker images
```
### 2. Check for running Docker images
```
docker ps
```
### 3. Check for all Docker images (Running and Not Running)
```
docker ps -a
```
### 4. Install docker images from dockerhub
1. Search for the docker image you want to install on docker hub
2. Pull the docker images on your machine/ server.
```
docker pull [image name]:[image tag]
```
#### Notes: 
If you do not specify your tag, the latest tag will be used.
```
docker pull [image name]
```
#### For example:
The following will pull tag **1.23** of **nginx** from docker.
```
docker pull nginx:1.23
```
### 5. Selecting and running an image
1. Find the image you would like to run
```
docker images
```
2. Run the selected image with the **image name** and **image tag**
```
docker run [image name]:[image tag]
```
#### Running a docker image without entering its screen.
Using **-d** will detact you from the docker [screen](https://phoenixnap.com/kb/how-to-use-linux-screen-with-commands#:~:text=Linux%20Screen%20provides%20users%20an,Screen%20on%20a%20Linux%20system) and thus allowing you to continue using the command prompt. 
- Running the above command will keep you within the image screen.**CTRL+C** will bring you out of the screen but at the same time kill the process of the image you are trying to run.
```
docker run -d [image name]:[image tag]
```

#### View the logs of docker image running in detach
1. Find the container ID or Name of your docker image
```
docker ps
```
2. Enter the following command
You may use **[container ID]** or **[container name]**
```
docker logs [container ID]
```
### 6. Run an image without pulling from docker
You could still run an immage that have not been pull from docker as the run command will automatically detect the missing image and pull from docker. You can simply get the **image name** and **image tag** from docker hub and type in the following command.
```
docker run [image name]:[image tag]
```
### 7. Stop Docker container
1. Find the docker container ID or Name
```
docker ps
```
2. Stop the docker container
You may use **[container ID]** or **[container name]**
```
docker stop [container ID]
```
### 8. Restart Docker container
1. Find the docker container ID or Name
```
docker ps -a
```
2. Restart the docker container
You may use **[container ID]** or **[container name]**
```
docker start [container ID]
```
***
## Exposing container port to the host (Server/ Machine) port
The process is called **port binding**.
### 1. Stop all docker container if exist.
```
docker stop [container ID]
```
### 2. Run the docker container with port specified.
Use **-p** or **--publish** command
- Host port is the port you want to bind to on your machine
- Container port is the port the program will run in. (Verify the program setting)
```
docker run -d -p [Host Port]:[Container Port] [image name]:[image tag]
```
***
## Naming a docker container
Use **--name** command when running the docker container
```
docker run --name [The Name You Want] -d [image name]:[image tag]
```
***
## Building a docker image
The building of a docker image requires a **Dockerfile**
### 1. Create a docker image
Create a docker file within the root of your file folder
``` 
cd /path/to/folder
sudo gedit /path/to/folder/DockerFile
```
#### 1.1. Image has to be executed in an order
**Base image** >> **Server/Compiler Image** >> **Your Program**
#### Example:
**NodeJS** >> **Tomcat** >> **Your Program**
### 2. Build your docker file
```
// Inform docker to build from a base image found in docker hub
FROM [base image name]:[base image tag]

// Copy source code of your program from local drive to the container
// COPY [Source] [Destination]
COPY package.json /app/         // Copy package.json from local drive to /app/ in container
COPY src /app/                  // Copy a directory

// Define a working directory for the program to be executed in
WORKDIR /app

// Run installation of dependencies
// In this case run install dependencies for NodeJS.
RUN npm install

// Last command in the docker file
// Inform docker to start the program, process or application
// CMD ["program", "parameter"]
// CMD ["node", server.js]      // Example of running nodejs
```
### 3. Build docker image
Within the same directory of your **docker file**, use the following command with **-t** or **--tag**.
- the "**.**" at the end refers to the current directory
```
docker build -t [Your New Image Name]:[Your New Image Tag] .
```
***
## Public and Private docker registry
Docker images pulled from docker are called **public registry** while private application that are developed privately are **public registry**.  
- In a private registry, you will have to authenticate before accessing the registry.
- The following site have support for **private registry**: **Amazon ECR**, **Google Container Registry**, **Nexus**, **Docker Hub**, and etc.
***
## Registry vs Repositroy
|Registry                               |Repository                   |
|---------------------------------------|-----------------------------|
|A Service that provides storage        |Collection of related images with the same name but different version |
|Hosted by third party or yourself      | |
|Contains a collection of repositories  | |