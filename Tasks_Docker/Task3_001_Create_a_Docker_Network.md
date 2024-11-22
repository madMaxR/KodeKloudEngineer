
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-22 11:51:00   
Finished: &nbsp;&nbsp;2024-11-22 12:12:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3_1: Create a Docker Network

## Requirements

The Nautilus DevOps team needs to set up several docker environments for different applications.  
One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later.  
Complete the task based on the following ticket description:

a. Create a docker network named as ecommerce on App Server 2 in Stratos DC.
b. Configure it to use macvlan drivers.
c. Set it to use subnet 192.168.0.0/24 and iprange 192.168.0.0/24.

------------------------------

## Steps
1. ssh stapp02
2. [steve@stapp02 ~]$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
81a26d0543e8   bridge    bridge    local
9f8b69977d8b   host      host      local
f6e7f6ec79b4   none      null      local
[steve@stapp02 ~]$ docker network create ecommerce --driver macvlan --subnet 192.168.0.0/24 --ip-range 192.168.0.0/24
e8a5a356dc8d2db541f53bfe2454bbd84ea7943c51c407e78009660c61711ffa
[steve@stapp02 ~]$ docker network ls
NETWORK ID     NAME        DRIVER    SCOPE
81a26d0543e8   bridge      bridge    local
e8a5a356dc8d   ecommerce   macvlan   local
9f8b69977d8b   host        host      local
f6e7f6ec79b4   none        null      local
[steve@stapp02 ~]$ docker network inspect ecommerce | head -15
[
    {
        "Name": "ecommerce",
        "Id": "e8a5a356dc8d2db541f53bfe2454bbd84ea7943c51c407e78009660c61711ffa",
        "Created": "2024-11-22T08:58:14.360204099Z",
        "Scope": "local",
        "Driver": "macvlan",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/24",
                    "IPRange": "192.168.0.0/24"
------------------------------

