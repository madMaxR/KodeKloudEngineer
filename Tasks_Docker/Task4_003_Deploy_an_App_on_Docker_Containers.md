
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-15 14:43:00  
Finished: &nbsp;&nbsp;2024-12-15 15:10:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4_3: Deploy an App on Docker Containers

## Requirements

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform.
The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment.
The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie.
Below are the details of the task:

  1. On App Server 1 in Stratos Datacenter create a docker compose file /opt/sysops/docker-compose.yml (should be named exactly).
  2. The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

    For web service:

    a. Container name must be php_host.

    b. Use image php with any apache tag.
    
    c. Map php_host container's port 80 with host port 5003
    
    d. Map php_host container's /var/www/html volume with host volume /var/www/html.
    
    For DB service:
    
    a. Container name must be mysql_host.
    
    b. Use image mariadb with any tag (preferably latest).
    
    c. Map mysql_host container's port 3306 with host port 3306
    
    d. Map mysql_host container's /var/lib/mysql volume with host volume /var/lib/mysql.
    
    e. Set MYSQL_DATABASE=database_host and use any custom user ( except root ) with some complex password for DB connections.

  3. After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:5003/


Note: Once you click on FINISH button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

------------------------------

## Steps

1. ```bash
   thor@jumphost ~$ ssh tony@stapp01
   ```
2. ```bash
   [tony@stapp01 ~]$ sudo su -
   ```
3. ```bash
   [root@stapp01 ~]# cd /opt/sysops/
   ```
4. ```bash
   [root@stapp01 ~]# vi docker-compose.yml
   [root@stapp01 ~]# cat docker-compose.yml 
   services:
     web:
       container_name: php_host
       image: php:7.2-apache
       ports:
         - "5003:80"
       volumes:
         - /var/www/html:/var/www/html
     
     db:
       container_name: mysql_host
       image: mariadb:latest
       ports:
         - "3306:3306"
       volumes:
         - /var/lib/mysql:/var/lib/mysql
       environment:
         MYSQL_DATABASE: database_host
         MYSQL_USER: custom_user
         MYSQL_PASSWORD: ComplexPassword123
         MYSQL_ROOT_PASSWORD: RootPassword123
   ```
5. ```bash
    [root@stapp01 sysops]# docker compose up -d
    [+] Running 24/24
     ✔ web Pulled                                                                                                        79.9s 
       ✔ 6ec7b7d162b2 Pull complete                                                                                       9.7s 
       ✔ db606474d60c Pull complete                                                                                      10.6s 
       ✔ afb30f0cd8e0 Pull complete                                                                                      31.2s 
       ✔ 3bb2e8051594 Pull complete                                                                                      40.4s 
       ✔ 4c761b44e2cc Pull complete                                                                                      47.4s 
       ✔ c2199db96575 Pull complete                                                                                      55.0s 
       ✔ 1b9a9381eea8 Pull complete                                                                                      59.9s 
       ✔ fd07bbc59d34 Pull complete                                                                                      62.4s 
       ✔ 72b73ab27698 Pull complete                                                                                      64.9s 
       ✔ 983308f4f0d6 Pull complete                                                                                      69.1s 
       ✔ 6c13f026e6da Pull complete                                                                                      71.5s 
       ✔ e5e6cd163689 Pull complete                                                                                      74.5s 
       ✔ 5c5516e56582 Pull complete                                                                                      77.4s 
       ✔ 154729f6ba86 Pull complete                                                                                      79.6s 
     ✔ db Pulled                                                                                                         55.8s 
       ✔ de44b265507a Pull complete                                                                                       9.3s 
       ✔ ca9b21d0c985 Pull complete                                                                                      10.1s 
       ✔ 041c753879ae Pull complete                                                                                      12.6s 
       ✔ e7b5137dc4b2 Pull complete                                                                                      13.8s 
       ✔ 655e5d2590bd Pull complete                                                                                      15.1s 
       ✔ 41b3170b5f12 Pull complete                                                                                      37.1s 
       ✔ 95adc28016bc Pull complete                                                                                      45.1s 
       ✔ 407e9d6eefb4 Pull complete                                                                                      55.5s 
    [+] Running 3/3
     ✔ Network sysops_default  Created                                                                                    0.1s 
     ✔ Container php_host      Started                                                                                   20.1s 
     ✔ Container mysql_host    Started
  ```
6. ```bash
   [root@stapp01 sysops]# curl localhost:5003/
   <html>
    <head>
        <title>Welcome to xFusionCorp Industries!</title>
    </head>

    <body>
        Welcome to xFusionCorp Industries!    </body>  
   ```





------------------------------



