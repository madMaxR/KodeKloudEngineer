
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-11 21:33:16  
Finished: &nbsp;&nbsp;2024-11-11 21:53:07

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 5: Troubleshoot Docker Container Issue

## Requirements

An issue has arisen with a static website running in a container named nautilus on App Server 1.
To resolve the issue, investigate the following details:
  1) Check if the container's volume `/usr/local/apache2/htdocs` is correctly mapped with the host's volume `/var/www/html`.
  2) Verify that the website is accessible on host port 8080 on App Server 1.
    Confirm that the command curl `http://localhost:8080/` works on App Server 1.

------------------------------

## Steps

1) ```bash
   [tony@stapp01 ~]$ docker ps -a
   CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS                     PORTS     NAMES
   b7e0a6e82aeb   httpd     "httpd-foreground"   3 minutes ago   Exited (0) 3 minutes ago             nautilus
   ```
2) ```bash
   [tony@stapp01 ~]$ docker logs nautilus
   AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.12.0.2. Set the 'ServerName' directive globally to suppress this message
   AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.12.0.2. Set the 'ServerName' directive globally to suppress this message
   [Mon Nov 11 18:31:51.812692 2024] [mpm_event:notice] [pid 1:tid 1] AH00489: Apache/2.4.62 (Unix) configured -- resuming normal operations
   [Mon Nov 11 18:31:51.812823 2024] [core:notice] [pid 1:tid 1] AH00094: Command line: 'httpd -D FOREGROUND'
   [Mon Nov 11 18:31:51.842953 2024] [mpm_event:notice] [pid 1:tid 1] AH00492: caught SIGWINCH, shutting down gracefully
   ```
3) ***To remove the Docker Container, stop it first***
   ```bash
   [tony@stapp01 ~]$ docker stop nautilus
   nautilus
   ```
5) ```bash
   [tony@stapp01 ~]$ docker rm nautilus
   nautilus
   ```
6) ```bash
   [tony@stapp01 ~]$ docker ps -a
   CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
   ```
7) **docker run [OPTIONS] IMAGE[:TAG] [COMMAND]**

   **-d**, --detach                      - Run in the background

   **-p**, HOST_PORT:CNTNR_PORT          - Expose a port or a range of ports

   **--name**                            - Assign a name to the container

   **-v**, --volume [HOST_SRC:]CNTR_DEST - Mount a volume between host and the container file system

   ```bash
   [tony@stapp01 ~]$ docker run -d -p 8080:80 --name nautilus -v /var/www/html:/usr/local/apache2/htdocs httpd:latest
   dc44a83af901e76d367ed5d761b70267468a4a77aac111c934df8967a4054192
   ```
9) ```bash
   [tony@stapp01 ~]$ curl http://localhost:8080/
   Welcome to xFusionCorp Industries!
   ```
   
   ------------------------------
