
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-26 17:18:00   
Finished: &nbsp;&nbsp;2024-11-26 17:25:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3_3: Docker Ports Mapping

## Requirements

The Nautilus DevOps team is planning to host an application on a nginx-based container.  
There are number of tickets already been created for similar tasks.  
One of the tickets has been assigned to set up a nginx container on Application Server 2 in Stratos Datacenter.  
Please perform the task as per details mentioned below:

a. Pull `nginx:alpine-perl` docker image on Application Server 2.

b. Create a container named `cluster` using the image you pulled.

c. Map host port `3004` to container port `80`. Please keep the container in running state.

------------------------------

## Steps
1. ssh stapp02
2. ```bash
   [steve@stapp02 ~]$ docker pull nginx:alpine-perl
   ```
3. ```bash
   [steve@stapp02 ~]$ docker container run -d --name cluster -p 3004:80 nginx:alpine-perl
   62daa67e19578048e262b3a3b454737c3331685b23917c02d6d9b790e337d551
   ```
4. ```bash
   [steve@stapp02 ~]$ docker ps
   CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS                  NAMES
   62daa67e1957   nginx:alpine-perl   "/docker-entrypoint.…"   23 seconds ago   Up 20 seconds   0.0.0.0:3004->80/tcp   cluster
   ```
5. ```bash
   [steve@stapp02 ~]$ curl -I localhost:3004
   HTTP/1.1 200 OK
   Server: nginx/1.27.2
   Date: Tue, 26 Nov 2024 14:21:43 GMT
   Content-Type: text/html
   Content-Length: 615
   Last-Modified: Wed, 02 Oct 2024 16:07:39 GMT
   Connection: keep-alive
   ETag: "66fd6fcb-267"
   Accept-Ranges: bytes
   
   ```
   
------------------------------



