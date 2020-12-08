# LFCS Module 5: Managing Virtualization

## Lesson 23: Working with Virtual Machines


## Lesson 24: Working with Containers
### Understanding Docker
`sudo apt install docker.io`  
`sudo systemctl start docker`  
The `docker` service must be running. 
Use `docker search` to search a container.  
Use `docker images` to list images.  
Use `docker pull` to pull an image to the local image store.  
Use `docker run` to run your container and start a command within.  
* `docker run nginx`
Use `docker create` to create a new container.  
Use `docker exec` to run a command in an already running container.  

Docker runs on the 172.17 network. So to scan for available IPs on that network run, `nmap -sn 172.17.0.0/24`. Note: `172.17.0.1` is the gateway for the docker network.  

### Using Storage with Docker containers
Storage can be mounted in a Docker container, but that ties the container to the host where the storage is accessible.  
`docker run --rm -it -v /data:/data ubuntu:latest /bin/bash`  
