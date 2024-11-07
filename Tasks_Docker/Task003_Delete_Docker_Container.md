
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-07 20:50:16  
Finished: &nbsp;&nbsp;2024-11-07 21:11:07

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 3: Delete Docker Container

## Requirements

A container named kke-container was created by one of the Nautilus project developers on App Server 3. 
It was solely for testing purposes and now requires deletion. 
Execute the following task:

- Delete the kke-container on App Server 3 in Stratos DC.

------------------------------

## Steps

1) ssh to stapp03 (172.16.238.12)
2) ```bash
   docker ps -a
   ```
3) ```bash
   docker stop kke-container
   ```
4) ```bash
   docker rm kke-container
   ```
5) ```bash
   docker ps -a
   ```
   
   ------------------------------
