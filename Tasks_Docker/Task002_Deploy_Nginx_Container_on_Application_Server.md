
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-07 19:20:16  
Finished: &nbsp;&nbsp;2023-05-29 19:38:25

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 2: Deploy Nginx Container on Application Server

## Requirements

Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 1. Please complete the task as per details given below:

On Application Server 1 create a container named nginx_1 using image nginx with alpine tag and make sure container is in running state.

------------------------------

## Steps

1) ssh to stapp01 (172.16.238.10)
2) ```bash
   docker pull nginx:alpine
   ```
3) ```bash
   docker run -d --name nginx_1 nginx:alpine
   ```
4) ```bash
   docker ps
   ```

   ------------------------------
