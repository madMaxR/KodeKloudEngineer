
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-18 20:33:00   
Finished: &nbsp;&nbsp;2024-11-18 21:19:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 2_5: Write a Docker File

## Requirements

As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects.
Several of the initial testing requirements are already been shared with DevOps team.
Therefore, create a docker file `/opt/docker/Dockerfile` (please keep D capital of Dockerfile) on `App server 3` in Stratos DC and configure to build an image with the following requirements:

a. Use `ubuntu` as the base image.
b. Install `apache2` and configure it to work on `8082` port. (do not update any other Apache configuration settings like document root etc).

------------------------------

## Steps
1. ssh banner@172.16.238.12 (stapp03)
2. ```bash
   [banner@stapp03 ~]$ sudo su -
   ```
3. ```bash
   [root@stapp03 ~]# touch /opt/docker/Dockerfile
   ```
5. ```bash
   [root@stapp03 ~]# vi /opt/docker/Dockerfile
   ```
   
   ```dockerfile
   FROM ubuntu
   RUN apt-get update && apt-get install -y apache2
   RUN echo "Listen 8082" >> /etc/apache2/ports.conf
   RUN sed -i 's/Listen 80/Listen 8082/g' /etc/apache2/sites-available/000-default.conf
   EXPOSE 8082
   CMD ["apachectl", "-D", "FOREGROUND"]
   ```
6. ```bash
   [root@stapp03 ~]# docker pull ubuntu
   Using default tag: latest
   latest: Pulling from library/ubuntu
   afad30e59d72: Pull complete 
   Digest: sha256:278628f08d4979fb9af9ead44277dbc9c92c2465922310916ad0c46ec9999295
   Status: Downloaded newer image for ubuntu:latest
   docker.io/library/ubuntu:latest
   ```
7. ```bash
   [root@stapp03 ~]# docker build -t apache-img /opt/docker/
   [+] Building 32.7s (8/8) FINISHED                                                                           docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 288B                                                                                  0.0s
    => [internal] load metadata for docker.io/library/ubuntu:latest                                                      0.0s
    => [internal] load .dockerignore                                                                                     0.0s
    => => transferring context: 2B                                                                                       0.0s
    => [1/4] FROM docker.io/library/ubuntu:latest                                                                        0.0s
    => [2/4] RUN apt-get update && apt-get install -y apache2                                                           20.7s
    => [3/4] RUN echo "Listen 8082" >> /etc/apache2/ports.conf                                                           2.8s 
    => [4/4] RUN sed -i 's/Listen 80/Listen 8082/g' /etc/apache2/sites-available/000-default.conf                        4.8s 
    => exporting to image                                                                                                4.3s 
    => => exporting layers                                                                                               4.3s 
    => => writing image sha256:502d0a20b20d48471e2bb8c569a3a33b6ed9e8d03ef187343bd16ab52bc95ccf                          0.0s 
    => => naming to docker.io/library/apache-img
   ```
8. Run the container from the image and check running containers
   ```bash
   [root@stapp03 ~]# docker run -d -p 8082:8082 apache-img
   2991fdab5c6583234c330e4c13b7f9c4b2f9af7ecbe71b2de951c11938d718ab
   [root@stapp03 ~]# docker ps -a
   CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS         PORTS                    NAMES
   2991fdab5c65   apache-img   "apachectl -D FOREGR…"   16 seconds ago   Up 7 seconds   0.0.0.0:8082->8082/tcp   loving_pasteur
   ```
9. 
   ```bash
   [root@stapp03 ~]# curl -I http://localhost:8082
   HTTP/1.1 200 OK
   Date: Thu, 21 Nov 2024 18:55:25 GMT
   Server: Apache/2.4.58 (Ubuntu)
   Last-Modified: Thu, 21 Nov 2024 18:48:59 GMT
   ETag: "29af-62770b82b4ba4"
   Accept-Ranges: bytes
   Content-Length: 10671
   Vary: Accept-Encoding
   Content-Type: text/html
   ```

   [//]: # (You have successfully completed the challenge. Results have been saved. Ref ID:64072036741b204d59fbe9c8)
------------------------------
