
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-05 20:50:00  
Finished: &nbsp;&nbsp;2024-11-05 21:35:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1: Install Docker Package

## Requirements

Last week the Nautilus DevOps team met with the application development team and decided to containerize several of their applications. The DevOps team wants to do some testing per the following:

- Install **docker-ce** and **docker-compose** packages on **App Server 1**.

- Start docker service.

------------------------------

## Steps
Решение:

Login to the App server and switch to root.

```bash
sshpass -p  '***********' ssh -o StrictHostKeyChecking=no tony@172.16.238.10
sudo su -
```
Download the docker-compose binary. Make the binary executable. 

```bash
curl -L "https://github.com/docker/compose/releases/tag/v2.30.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  
```
*если всплыла ошибка
````bash
./docker-compose: line 8: syntax error near unexpected token `newline' ./docker-compose: line 8: `<!DOCTYPE html>'
````
, то нужно выбрать тип системы и архитектуру руками*

```bash
chmod +x /usr/local/bin/docker-compose 
```

Next, download docker. Afterwards, verify that docker is successfully installed.

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo  
yum install -y docker-ce docker-ce-cli containerd.io
```
```bash
[root@stapp01 ~]# rpm -qa |grep docker 
docker-buildx-plugin-0.10.5-1.el7.x86_64
docker-ce-24.0.2-1.el7.x86_64
docker-compose-plugin-2.18.1-1.el7.x86_64
docker-ce-cli-24.0.2-1.el7.x86_64
docker-ce-rootless-extras-24.0.2-1.el7.x86_64
```

Enable and start the service.

```bash
systemctl enable docker  
systemctl start  docker
systemctl status docker
```

Verify.

```bash
[root@stapp01 ~]# docker --version 

[root@stapp01 ~]# docker-compose --version
```


------------------------------
