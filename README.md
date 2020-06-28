# learningDocker
Docker concepts

**Docker Networks:**
There are three types of docker networks.

1. Bridge network(default) - docker run ubuntu
2. None network - docker run --network=none ubuntu
3. host network - docker run --network=host ubuntu

**Bridge Network**
When a docker container is created without specifying any particular network, its assigned under default network which is bridge network.This is private internal network on docker host in range of 172.17... series.
Containers within bridge network can talk to each other using internal ip address. If these
containers were to be accessed by outside bridge network then that can be done by mapping its port to any of the docker host port(port for machine on which docker is running) by using -p option. Another way is to associate a host port with the container.

**None network:** When a container is created with none, its not associated with any network and it cannot be accessed by anyone , or it cannot access any other container. They run in an isolated network.

**host network:**

Container created with host option is hosted on a host network not docker internal network. Like a container can be hosted on 5000 port on host network. This way without port mapping container can be accessed by outside networks.

- By default docker will create one bridge network but we can create another one using below command :

```
docker network create 
--driver bridge 
--subnet 182.18.0.0/16
custom-isolated-network
```

`docker network ls` -> to list network list.

`docker inspect containerid` -> to verify a container's network details.

Embedded DNS server: Docker has DNS server running on ip 127.0.0.11. Every container has a container name which can be used to resolve a container by embedded DNS. So if a webserver wants to connect to a mysql container, it can does to by container name and in turn it uses DNS to resolve the ip address using DNS server.

**Docker Registry**
Docker registry is a cloud space where images are maintained and stored. If no path is mentioned specifically by default registry is docker.io. For example if you are pulling a nginx image then actully path is: docker.io/nginx/nginx : here 1st nginx is user account, 2nd is image name. 

But this is public registry. Likewise docker's own , there is google's public registry as well gcr.io where kubernetes images are available. 
One can create its own private registry as well and to access that, you need to first login using `docker login` command. When you create an account in AWS of GCP (Google cloud), they by default provide a registry to maintain images. 

Deploy private registry:

Docker registry docker.io is itself a image called registry. If we want to run an instance of the private registry tin local then execute below command:
`Docker run -d --name registry -p 5000:5000 registry:2 ` - will run and deploy private registry to port 5000 on docker host.

Tagging your own image to docker registry by using Docker image tag command and attach the image to private registry url:
` Docker image tag my-image localhost:5000/my-image `
Pushing the image to local private registry:
`docker push localhost:5000/my-image`
Pulling image from same host or different host:
`Docker pull localhost:5000/my-image` or `Docker pull 192.168.56.100:5000/my-image `


**Docker install on windows can be done through 2 ways:**
1. Docker toolbox - Toolbox consists of Oracle virtual box, A linux system and a docker on that linux system. Docker toolbox does nothing but
installs a virutal linux system on your windows machine so that it can support linux images to run. Windows 7 , 64 bit system is required for Docker toolbox.
2. Docker Desktop - Docker toolbox is legacy system and is required for older windows. Docker desktop is similar but it uses Microsoft hiper-v for virualization instead of oracle virtual box. MS hiper-v is only supported by windows enterprise version or professional version as they come with by default hiper-v support. 

Now above options supports running linux containers on windows host machine. Windows server 2016 support windows containers. 

Windows containers are of two types:

1. Windows server (OS kernel is shared between containers)
2. Hiper-v isolation where a highly optimized OS is created or each container and each container uses its own share of OS. 


