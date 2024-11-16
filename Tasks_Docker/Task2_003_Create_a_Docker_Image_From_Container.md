One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:


a. Create an image `blog:datacenter` on `Application Server 1` from a container `ubuntu_latest` that is running on same server.

1) ssh tony@172.16.238.10 

[tony@stapp01 ~]$ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS          PORTS     NAMES
7af2f09cf614   ubuntu    "/bin/bash"   About a minute ago   Up 57 seconds             ubuntu_latest

[tony@stapp01 ~]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    fec8bfd95b54   4 weeks ago   78.1MB

[tony@stapp01 ~]$ docker commit ubuntu_latest blog:datacenter
sha256:934222c007ee8f273e7830ca6ac5df00e803404627598440790bb1a3d50e3b85

[tony@stapp01 ~]$ docker images
REPOSITORY   TAG          IMAGE ID       CREATED          SIZE
blog         datacenter   934222c007ee   21 seconds ago   121MB
ubuntu       latest       fec8bfd95b54   4 weeks ago      78.1MB
