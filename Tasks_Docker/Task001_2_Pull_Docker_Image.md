
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-12 19:14:00
Finished: &nbsp;&nbsp;2024-11-12 19:53:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1_2: Pull Docker Image

## Requirements

Nautilus project developers are planning to start testing on a new project.
As per their meeting with the DevOps team, they want to test containerized environment application features.
As per details shared with DevOps team, we need to accomplish the following task:

a. Pull `busybox:musl` image on App Server 1 in Stratos DC and re-tag (create new tag) this image as `busybox:blog`.

------------------------------

## Steps

1) ssh to stapp01 (172.16.238.10)
2) ```bash
   [tony@stapp01 ~]$ docker images
   REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
   ```
3) ```bash
   [tony@stapp01 ~]$ docker pull busybox:musl
   0521bfaffa2c: Pull complete 
   Digest: sha256:b6252cc4d3a3a702284d828b89cf99d902fad4b00b4aebf2299aa15bfeae54bf
   Status: Downloaded newer image for busybox:musl
   docker.io/library/busybox:mus
   ```
5) ```bash
   [tony@stapp01 ~]$ docker images
   REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
   busybox      musl      32006d2bebf5   6 weeks ago   1.4MB
   ```
6) ```bash
   [tony@stapp01 ~]$ docker ps -a
   CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
   ```
7) ```bash
   [banner@stapp03 ~]$ sudo docker tag busybox:musl busybox:blog
   ```
9) ```bash
   [banner@stapp03 ~]$ sudo docker images
   REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
   busybox      blog      32006d2bebf5   6 weeks ago   1.4MB
   busybox      musl      32006d2bebf5   6 weeks ago   1.4MB 
   ```
   
   ------------------------------