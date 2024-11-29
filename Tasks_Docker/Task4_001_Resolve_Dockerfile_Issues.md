
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-29 22:50:00  
Finished: &nbsp;&nbsp;2024-11-29 23:25:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4_1: Resolve Dockerfile Issues

## Requirements

The Nautilus DevOps team is working to create new images per requirements shared by the development team.
One of the team members is working to create a `Dockerfile` on `App Server 2` in Stratos DC.
While working on it she ran into issues in which the docker build is failing and displaying errors.
Look into the issue and fix it to build an image as per details mentioned below:

a. The `Dockerfile` is placed on `App Server 3` under `/opt/docker` directory.

b. Fix the issues with this file and make sure it is able to build the image.

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, `index.html`.

`Note:` Please note that once you click on `FINISH` button all existing images, the containers will be destroyed and new image will be built from your `Dockerfile`.

------------------------------

## Steps

1. ```bash
   thor@jumphost ~$ ssh steve@stapp02
   ```
2. ```bash
   [steve@stapp02 ~]$ sudo su -
   ```
3. ```bash
   [root@stapp02 ~]# cd /opt/docker/
   [root@stapp02 docker]# ls -lah
   total 20K
   drwxrwxrwx 4 root root 4.0K Nov 29 20:31 .
   drwxr-xr-x 1 root root 4.0K Nov 29 20:31 ..
   -rw-r--r-- 1 root root  504 Nov 29 20:31 Dockerfile
   drwxr-xr-x 2 root root 4.0K Nov 29 20:02 certs
   drwxr-xr-x 2 root root 4.0K Nov 29 20:02 html
   ```
4. ```bash
   [root@stapp02 docker]# docker build -t dockerfile_img .
   [+] Building 0.2s (12/12) FINISHED                                                                          docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 543B                                                                                  0.0s
    => [internal] load metadata for docker.io/library/httpd:2.4.43                                                       0.0s
    => [internal] load .dockerignore                                                                                     0.0s
    => => transferring context: 2B                                                                                       0.0s
    => [1/8] FROM docker.io/library/httpd:2.4.43                                                                         0.1s
    => [internal] load build context                                                                                     0.0s
    => => transferring context: 2B                                                                                       0.0s
    => CACHED [2/8] RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf                            0.0s
    => CACHED [3/8] RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf                     0.0s
    => CACHED [4/8] RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.con  0.0s
    => CACHED [5/8] RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf                           0.0s
    => **ERROR** [6/8] COPY /server.crt /usr/local/apache2/conf/server.crt                                                   0.0s
    => **ERROR** [7/8] COPY /server.key /usr/local/apache2/conf/server.key                                                   0.0s
    => **ERROR** [8/8] COPY ./index.html /usr/local/apache2/htdocs/                                                          0.0s
   ------
    > [6/8] COPY /server.crt /usr/local/apache2/conf/server.crt:
   ------
   ------
    > [7/8] COPY /server.key /usr/local/apache2/conf/server.key:
   ------
   ------
    > [8/8] COPY ./index.html /usr/local/apache2/htdocs/:
   ------
   Dockerfile:15
   --------------------
     13 |     COPY /server.key /usr/local/apache2/conf/server.key
     14 |     
     15 | >>> COPY ./index.html /usr/local/apache2/htdocs/
   --------------------
   ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref e89f6470-6b4e-4862-9c85-710d7e72963a::9zc5ei3n7pkjb9503of14mpeo: "/index.html": not found
   ```
5. ```bash
   [root@stapp02 docker]# cat Dockerfile 
   FROM httpd:2.4.43
  
   RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
  
   RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
  
   RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
  
   RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf
  
   COPY /server.crt /usr/local/apache2/conf/server.crt
  
   COPY /server.key /usr/local/apache2/conf/server.key
  
   COPY ./index.html /usr/local/apache2/htdocs/
   ```
6. a) Некорректный путь в командах `sed`  
   b) Отсутствие директории `certs`  
   c) Отсутствие директории `html`  
   d) Рекомендуется указать порт  
   
   ```bash
   [root@stapp02 docker]# vi Dockerfile 
   [root@stapp02 docker]# cat Dockerfile 
   FROM httpd:2.4.43
  
   RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
  
   RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
  
   RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
  
   RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf
  
   COPY certs/server.crt /usr/local/apache2/conf/server.crt
  
   COPY certs/server.key /usr/local/apache2/conf/server.key
  
   COPY html/index.html /usr/local/apache2/htdocs/index.html
  
   EXPOSE 8080
   ```
7. ```bash
   [root@stapp02 docker]# docker build -t dockerfile_img .
   [+] Building 12.3s (13/13) FINISHED                                                                         docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 637B                                                                                  0.0s
    => [internal] load metadata for docker.io/library/httpd:2.4.43                                                       0.0s
    => [internal] load .dockerignore                                                                                     0.0s
    => => transferring context: 2B                                                                                       0.0s
    => [1/8] FROM docker.io/library/httpd:2.4.43                                                                         0.0s
    => [internal] load build context                                                                                     0.0s
    => => transferring context: 3.19kB                                                                                   0.0s
    => CACHED [2/8] RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf                            0.0s
    => [3/8] RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf         1.9s
    => [4/8] RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/co  1.7s
    => [5/8] RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf               1.7s
    => [6/8] COPY certs/server.crt /usr/local/apache2/conf/server.crt                                                    1.0s
    => [7/8] COPY certs/server.key /usr/local/apache2/conf/server.key                                                    1.2s
    => [8/8] COPY html/index.html /usr/local/apache2/htdocs/index.html                                                   1.1s
    => exporting to image                                                                                                3.7s
    => => exporting layers                                                                                               3.6s
    => => writing image sha256:2973615c381ba9cfbd9d3058ab1116d8605b7382be7a3237448af4243ab4abf7                          0.0s
    => => naming to docker.io/library/dockerfile_img
   ```
8. ```bash
   [root@stapp02 docker]# docker images
   REPOSITORY       TAG       IMAGE ID       CREATED              SIZE
   dockerfile_img   latest    2973615c381b   About a minute ago   166MB
   ```
9. ```bash
   [root@stapp02 docker]# docker run -d -p 8080:8080 dockerfile_img
   323444664fae33b48df3d524dbe5c813d0e7de8e909c973c28ffedbef239a05a
   ```
10. ```bash
    [root@stapp02 docker]# curl localhost:8080
    This Dockerfile works!
    ```
------------------------------



