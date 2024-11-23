
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-23 12:22:00   
Finished: &nbsp;&nbsp;2024-11-22 12:48:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3_2: Docker Volumes Mapping

## Requirements

The Nautilus DevOps team is testing applications containerization,  
which is supposed to be migrated on docker container-based environments soon.  
In today's stand-up meeting one of the team members has been assigned a task  
to create and test a docker container with certain requirements.  
Below are more details:

a. On `App Server 1` in Stratos DC pull `nginx` image (preferably `latest` tag but others should work too).  
b. Create a new container with name `news` from the image you just pulled.  
c. Map the host volume `/opt/devops` with container volume `/tmp`.  
   There is an `sample.txt` file present on same server under `/tmp`; copy that file to `/opt/devops`.  
   Also please keep the container in running state.  

------------------------------

## Steps
1. ssh stapp01
2. ```bash
   [tony@stapp01 ~]$ docker images
   REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
   ```
3. ```bash
   [tony@stapp01 ~]$ docker pull nginx:latest 
   latest: Pulling from library/nginx
   2d429b9e73a6: Pull complete 
   9b1039c85176: Pull complete 
   9ad567d3b8a2: Pull complete 
   773c63cd62e4: Pull complete 
   1d2712910bdf: Pull complete 
   4b0adc47c460: Pull complete 
   171eebbdf235: Pull complete 
   Digest: sha256:bc5eac5eafc581aeda3008b4b1f07ebba230de2f27d47767129a6a905c84f470
   Status: Downloaded newer image for nginx:latest
   docker.io/library/nginx:latest
   ```
4. ```bash
   [tony@stapp01 ~]$ sudo cp /tmp/sample.txt /opt/devops/
   ```
5. ```bash
   [tony@stapp01 ~]$ docker run --name news -v /opt/devops:/tmp -d -it nginx:latest
   f3b7aeec8bde342f5b3ea51251047c8854cbc60497d68a51a1d5ecee0feb2c0d
   ```
6. ```bash
   [tony@stapp01 ~]$ docker ps
   CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
   f3b7aeec8bde   nginx:latest   "/docker-entrypoint.…"   24 seconds ago   Up 21 seconds   80/tcp    news
   [tony@stapp01 ~]$ docker exec -it news ls -la /tmp
   total 12
   drwxr-xr-x  2 root root 4096 Nov 23 09:30 .
   drwxr-xr-x 18 root root 4096 Nov 23 09:31 ..
   -rw-r--r--  1 root root   23 Nov 23 09:30 sample.txt
   ```
------------------------------


