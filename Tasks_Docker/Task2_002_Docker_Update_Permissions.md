
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-13 22:01:00   
Finished: &nbsp;&nbsp;2024-11-13 22:25:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 2_2: Docker Update Permissions

## Requirements

One of the Nautilus project developers need access to run docker commands on App Server 1.
This user is already created on the server. Accomplish this task as per details given below:
 - User `ammar` is not able to run docker commands on App Server 1 in Stratos DC, make the required changes so that this user can run docker commands without **`sudo`**.

------------------------------

## Steps

1) ssh to stapp01 (172.16.238.10)
2) ```bash
   [tony@stapp01 ~]$ sudo getent group docker

   We trust you have received the usual lecture from the local System
   Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

   [sudo] password for tony: 
   docker:x:996:tony
   ```
3) ```bash
   [tony@stapp01 ~]$ sudo usermod -aG docker ammar
   ```
4) ```bash
   [tony@stapp01 ~]$ sudo getent group docker
   docker:x:996:tony,ammar
   ```
5) ```bash
   [tony@stapp01 ~]$ sudo su
   [root@stapp01 tony]# su - ammar
   [ammar@stapp01 ~]$ docker --version
   Docker version 26.1.3, build b72abbb
   [ammar@stapp01 ~]$ docker images
   REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
   [ammar@stapp01 ~]$ docker ps -a
   CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
   ```

   
   ------------------------------
