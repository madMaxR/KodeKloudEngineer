
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-10 20:40:16  
Finished: &nbsp;&nbsp;2024-11-10 21:01:07

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4: Copy File to Docker Container

## Requirements

The Nautilus DevOps team possesses confidential data on App Server 1 in the Stratos Datacenter.
A container named ubuntu_latest is running on the same server.
Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /usr/src/.
Ensure the file is not modified during this operation.

------------------------------

## Steps

1) ssh to stapp01 (172.16.238.10)
2) ```bash
   docker ps -a
   ```
3) ```bash
   docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src/
   ```
4) **docker exec** - to Access the shell of Docker Container
   ```bash
   [tony@stapp01 ~]$ docker exec ubuntu_latest ls -ltr /usr/src/
   total 4
   -rw-r--r-- 1 root root 105 Nov 10 17:34 nautilus.txt.gpg
   ```
   
   ------------------------------
