
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-28 20:32:00  
Finished: &nbsp;&nbsp;2024-11-28 20:56:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3_5: Write a Docker Compose File

## Requirements

The Nautilus application development team shared static website content that needs to be hosted on the `httpd` web server using a containerised platform.
The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines.
Below are the details:

a. On `App Server 3` in Stratos DC create a container named `httpd` using a docker compose file `/opt/docker/docker-compose.yml` (please use the exact name for file).

b. Use `httpd` (preferably `latest` tag) image for container and make sure container is named as `httpd`; you can use any name for service.

c. Map `80` number port of container with port `8084` of docker host.

d. Map container's `/usr/local/apache2/htdocs` volume with `/opt/itadmin` volume of docker host which is already there. (please do not modify any data within these locations).

------------------------------

## Steps

1. ssh stapp03
2. ```bash
   [banner@stapp03 docker]$ sudo su -
   ```
3. 
   ```bash
   [root@stapp03 ~]# cd /opt/docker/
   ```
4. ```bash
   [root@stapp03 docker]# touch docker-compose.yml
   ```
5. ```bash
   [root@stapp03 docker]# vi docker-compose.yml
   ```
6. ```bash
   [root@stapp03 docker]# cat docker-compose.yml 
   services:
     httpd-container:
       image: httpd:latest
       container_name: httpd
       ports:
         - "8084:80"
       volumes:
         - /opt/itadmin:/usr/local/apache2/htdocs
   ```
7. ```bash
   [root@stapp03 docker]# docker compose up -d
   [+] Running 7/7
    ✔ httpd-container Pulled                                                                                             9.6s 
      ✔ 2d429b9e73a6 Pull complete                                                                                       3.8s 
      ✔ d675ed392a91 Pull complete                                                                                       4.3s 
      ✔ 4f4fb700ef54 Pull complete                                                                                       4.9s 
      ✔ 3ed0d9182dde Pull complete                                                                                       5.9s 
      ✔ 0062038102c9 Pull complete                                                                                       8.6s 
      ✔ 334a67c7f78b Pull complete                                                                                       9.4s 
   [+] Running 2/2
    ✔ Network docker_default  Created                                                                                    0.1s 
    ✔ Container httpd         Started                                                                                    2.5s 
   [root@stapp03 docker]# docker ps -a
   CONTAINER ID   IMAGE          COMMAND              CREATED         STATUS         PORTS                  NAMES
   e608aa882f02   httpd:latest   "httpd-foreground"   9 seconds ago   Up 6 seconds   0.0.0.0:8084->80/tcp   httpd
   ```
8. ```bash
   [root@stapp03 docker]# curl -I localhost:8084
   HTTP/1.1 200 OK
   Date: Thu, 28 Nov 2024 17:49:29 GMT
   Server: Apache/2.4.62 (Unix)
   Content-Type: text/html;charset=ISO-8859-1
   
   [root@stapp03 docker]# curl localhost:8084
   <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
   <html>
    <head>
     <title>Index of /</title>
    </head>
    <body>
   <h1>Index of /</h1>
   <ul><li><a href="index1.html"> index1.html</a></li>
   </ul>
   </body></html>
   ```
9. **`docker inspect`** - Return low-level information on Docker objects  
   **Usage:**	`docker inspect [OPTIONS] NAME|ID [NAME|ID...]`
   ```bash
   [root@stapp03 docker]# docker inspect httpd | grep -A 5 "\"Ports\":"
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8084"
                    }
   ```
10. ```bash
    [root@stapp03 docker]# docker inspect httpd | grep -A 8 Mounts
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/opt/itadmin",
                "Destination": "/usr/local/apache2/htdocs",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            }
    ```

[//]: # (You have successfully completed the challenge.Results have been saved. Ref ID:6407203e741b204d59fbe9d4)
------------------------------


