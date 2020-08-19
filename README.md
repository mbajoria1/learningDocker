# Learning Docker

## Docker concepts

Docker is a application build, ship & run tool which allows to write, build & deploy application faster. 

### Docker Client

      1. Helps to build, pull & run images
      2. Build, Pull, run commands does nothing but calls rest apis hosted on docker host.
      3. Client is stateless, state is maintained at docker host.
      
### Docker Host 

      1. Is the hosting application which has images & containers running on it. 
      2. It also has a Docker Daemon which keeps listening to incoming requests and serve to it.
      3. Docker host upon getting request to run an image, checks if image is present in local, if not present it pulls the image from docker hub registry. All this happends     
      behind the scene, as a developer we dont need to worry about it. 
      4. We can have docker host clusters insead of a single docker host to avoid single point of failure.

### Docker Registry

      1. Docker trusted registry can be used to store images.
      2. We also have docker hub registry where anyone can store & pull images from.
      
### Basic Docker Commands:

```Docker --version```  
- to check docker version

```Docker info```  
- provides information about docker environment

```Docker version```  
- provides client & server(Docker host) details

```Docker help``` 
- provides help related to different docker commands. 

```Docker container ls```
- provides list of containers

```Docker image ls```
- provides list of images

```docker container run -it nginx```
- run docker image in background mode/interactive mode

```docker container stop containerID or containerName```
- stop a running container

```docker rm contained id```
- remove container from docker host

```docker rmi imageid```
- remove image from local

```docker container run -d --name web -P jboss/wildfly```
- pulls jboss/wildfly image, runs it on docker host with a name web and map the port to local host in a default port. 
```docker container run -d --name web -p 8080:8080 jboss/wildfly```
- same as before but maps to localhost & port 8080


<br/>

## Docker Networks

There are three types of docker networks -

  1. Bridge network(default)
  2. `none` network 
  3. `host` network

<br/> 

### **Bridge Network:**

- When a docker container is created without specifying any particular network, its assigned under default network which is bridge network.This is private internal network on docker host in range of **172.17...** series.

  ```
    docker run ubuntu
  ```

- Containers within bridge network can talk to each other using internal ip address. If these containers were to be accessed by outside bridge network then that can be done by mapping its port to any of the docker host port(port for machine on which docker is running) by using **-p option**. Another way is to associate a host port with the container.

### **`none` network:** 

- When a container is created with `none`, its not associated with any network. Neither it can be accessed by anyone, nor can it access any other container. They run in an isolated network.

  ```
    docker run --network=none ubuntu
  ```

### **`host` network:**

- Container created with `host` option is hosted on a host network not docker internal network. Like a container can be hosted on 5000 port on host network. This way without port mapping container can be accessed by outside networks.

  ```
    docker run --network=host ubuntu
  ```
  
> **Note** : 
>
> - By default docker will create one bridge network but we can create another one using below command :
>
>   ```
>     docker network create 
>     --driver bridge 
>     --subnet 182.18.0.0/16
>     custom-isolated-network
>   ```
>
>  - List networks: `docker network ls` 
>  - Inspect a container's network:`docker inspect containerid`

<br/>

## Embedded DNS server: 

- Docker has **DNS server** running on ip **127.0.0.11**. 
- Every container has a container name which can be used to resolve a container by embedded DNS. So if a webserver wants to connect to a mysql container, it can do by providing the container name and in turn it uses DNS to resolve the ip address using DNS server.

<br/>

## Docker Registry

- Docker registry is a cloud space where docker images are maintained and stored.  
- If no path is specified the default registry is **docker.io**. For example if you are pulling a nginx image then actully path is: docker.io/nginx/nginx : here 1st nginx is user account, 2nd is image name. But this is public registry. 
- Likewise docker's own , there is **google's public registry** as well **gcr.io** where kubernetes images are available. 
- One can create its own private registry as well and to access that, you need to first login using `docker login` command. When you create an account in AWS or GCP (Google cloud), they by default provide a registry to maintain docker images. 

<br/>

## Deploy private registry:

- Docker registry docker.io is itself a image called registry. If we want to run an instance of the private registry in local then execute below command:
  ```
    docker run -d --name registry -p 5000:5000 registry:2 
  ``` 
  This will run and deploy private registry to port `5000` on docker host.

- Tagging your own image to docker registry by using Docker image tag command and attach the image to private registry url:
  ```
    docker image tag my-image localhost:5000/my-image 
  ```

- Pushing the image to local private registry:
  ```
    docker push localhost:5000/my-image
  ```

- Pulling image from same host or different host:
  ```
    docker pull localhost:5000/my-image 
    //or 
    docker pull 192.168.56.100:5000/my-image 
  ```

<br/>

## Docker install on windows 

- Docker install on Windows can be done by two ways:

  1. **Docker toolbox** - Toolbox consists of Oracle virtual box, A linux system and a docker on that linux system. Docker toolbox does nothing but
installs a virutal linux system on your windows machine so that it can support linux images to run. Windows 7 , 64 bit system is required for Docker toolbox.
  2. **Docker Desktop** - Docker toolbox is legacy system and is required for older windows. Docker desktop is similar but it uses Microsoft hiper-v for virualization instead of oracle virtual box. MS hiper-v is only supported by windows enterprise version or professional version as they come with by default hiper-v support. 

- Above options supports running linux containers on windows host machine. Windows server 2016 support Windows containers. Windows containers are of two types:

  1. Windows server (OS kernel is shared between containers)
  2. Hiper-v isolation where a highly optimized OS is created or each container and each container uses its own share of OS. 


