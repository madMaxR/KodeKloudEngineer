
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-27 19:30:00  
Finished: &nbsp;&nbsp;2024-11-27 19:46:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3_4: Save, Load and Transfer Docker Image

## Requirements

One of the DevOps team members was working on to create a new custom docker image on `App Server 1` in Stratos DC.
He is done with his changes and image is saved on same server with name `blog:datacenter`.
Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on `App Server 3`.
So we need to provide them that image on `App Server 3` in Stratos DC.

a. On `App Server 1` save the image `blog:datacenter` in an archive.

b. Transfer the image archive to `App Server 3`.

c. Load that image archive on `App Server 3` with same name and tag which was used on `App Server 1`.

`Note:` Docker is already installed on both servers; however, if its service is down please make sure to start it.

------------------------------

## Steps

1. ssh stapp01
2. ```bash
   [tony@stapp01 ~]$ docker images
   REPOSITORY   TAG          IMAGE ID       CREATED         SIZE
   blog         datacenter   21c224f4062f   2 minutes ago   121MB
   ubuntu       latest       fec8bfd95b54   6 weeks ago     78.1MB
   ```
3. Save the image in an archive file and then verify.   
   **`docker image save`** - Save one or more images to a tar archive (streamed to STDOUT by default)  
   `-o`, --output - write to a file, instead of STDOUT
   ```bash
   [tony@stapp01 ~]$ docker save -o /tmp/blog.tar blog:datacenter
   ```
5. ```bash
   [tony@stapp01 ~]$ ls -la /tmp/ | grep tar
   -rw------- 1 tony tony 123457024 Nov 27 16:35 blog.tar
   ```
6. ```bash
   [tony@stapp01 ~]$ sudo scp /tmp/blog.tar banner@stapp03:/tmp
   banner@stapp03's password: 
   blog.tar                       100%  118MB 141.5MB/s   00:00
   ```
7. ssh stapp03
8. ```bash
   [banner@stapp03 ~]$ ls -la /tmp/ | grep tar
   -rw------- 1 banner banner 123457024 Nov 27 16:38 blog.tar
   ```
9. ```bash
   [banner@stapp03 ~]$ docker load -i /tmp/blog.tar 
   27123a71e85e: Loading layer [==================================================>]  80.63MB/80.63MB
   778f8f524562: Loading layer [==================================================>]  42.81MB/42.81MB
   Loaded image: blog:datacenter
   ```
10. ```bash
   [banner@stapp03 ~]$ docker images
   REPOSITORY   TAG          IMAGE ID       CREATED          SIZE
   blog         datacenter   21c224f4062f   10 minutes ago   121MB
   ```

------------------------------

