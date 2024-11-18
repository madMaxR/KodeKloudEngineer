
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-18 20:33:00   
Finished: &nbsp;&nbsp;2024-11-18 21:12:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 2_4: Docker EXEC Operations

## Requirements

One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on `App Server 3` in Stratos Datacenter.
Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP.
Please complete the remaining work as per details given below:

a. Install `apache2` in `kkloud` container using apt that is running on `App Server 3` in Stratos Datacenter.
b. Configure Apache to listen on port `5003` instead of default `http` port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on `localhost`, 127.0.0.1, container ip, etc.
c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

------------------------------

## Steps

1) ssh to stapp03 (172.16.238.12)
2) ```bash
   [banner@stapp03 ~]$ docker ps -a
   CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
   3168794604a1   ubuntu:18.04   "/bin/bash"   3 minutes ago   Up 3 minutes             kkloud
   ```
   
   `docker container exec` - Execute a command in a running container          
   -i (interactive) - опция указывает Docker держать стандартный ввод (stdin) открытым, для ввода команд.           
   -t (tty) - эта опция подключает интерактивную оболочку с терминалом.               
   `/bin/bash` - запуск Bash-оболочки                
   
3) ```bash
   [banner@stapp03 ~]$ docker exec -it kkloud /bin/bash
   ```
4) ```bash
   root@3168794604a1:/# apt install apache2 -y
   ```
5) ```bash
   root@3168794604a1:/# cd /etc/apache2/
   root@3168794604a1:/etc/apache2# ls
   apache2.conf    conf-enabled  magic           mods-enabled  sites-available
   conf-available  envvars       mods-available  ports.conf    sites-enabled
   ```
6) ```bash
   root@3168794604a1:/etc/apache2# sed -i 's/Listen 80/Listen 5003/g' ports.conf
   ```
7) Ensure that the VirtualHost configuration in /etc/apache2/sites-available/000-default.conf also references port 5003:
   ```bash
   sed -i 's/:80/:5003/g' sites-available/000-default.conf
   ```
8) ```bash
   service apache2 restart
   * Restarting Apache httpd web server apache2 
   ```
   
   Если после команды restart вылезло предупреждение:      
   ```bash        
   AH00558: apache2: Could not reliably determine        
   the server's fully qualified domain name,          
   using 172.12.0.2. Set the 'ServerName' directive       
   globally to suppress this message
   ```
   
   Это говорит о том, что Apache не настроен на использование определенного имени хоста (ServerName),       
   поэтому он автоматически использует IP-адрес контейнера. Хотя это сообщение является предупреждением и      
   Apache все равно запускается, следует его устранить, настроив параметр ServerName:
  
   ```bash
   echo "ServerName localhost" >> /etc/apache2/apache2.conf
   ```
9) Проверка синтаксиса конфигурации:
   ```bash
   root@3168794604a1:/etc/apache2# apachectl configtest
   Syntax OK
   ```
10) ```bash
    root@3168794604a1:/etc/apache2# service apache2 restart
    * Restarting Apache httpd web server apache2             [ OK ]    
    root@3168794604a1:/etc/apache2# apachectl configtest
    Syntax OK
    ```
11) Keep the container in running state at the end:
    ```bash
    [banner@stapp03 ~]$ docker ps -a
    CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
    3168794604a1   ubuntu:18.04   "/bin/bash"   44 minutes ago   Up 44 minutes             kkloud
    ```

   # Ref ID:64072035741b204d59fbe9c6
   ------------------------------
