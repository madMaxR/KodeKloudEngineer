
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-11-30 12:47:00  
Finished: &nbsp;&nbsp;2024-11-30 13:21:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4_2: Resolve Docker Compose Issues

## Requirements

The Nautilus DevOps team is working to deploy one of the applications on `App Server 2` in Stratos DC.
Due to a misconfiguration in the docker compose file, the deployment is failing.
We would like you to take a look into it to identify and fix the issues.
More details can be found below:

a. `docker-compose.yml` file is present on `App Server 2` under `/opt/docker` directory.

b. Try to run the same and make sure it works fine.

c. Please do not change the `container names` being used. Also, do not update or alter any other valid config settings in the compose file or any other relevant data that can cause app failure.

`Note:` Please note that once you click on `FINISH` button all existing running/stopped containers will be destroyed, and your compose will be run.

------------------------------

## Steps

1. ```bash
   thor@jumphost ~$ ssh steve@stapp02
   ```
2. ```bash
   [steve@stapp02 ~]$ sudo su -
   ```
3. ```bash
   [root@stapp02 ~]# cd /opt/docker/
   [root@stapp02 docker]# docker compose up -d
   validating /opt/docker/docker-compose.yml: services.web Additional property port is not allowed
   ```
4. Ошибка `services.web Additional property port is not allowed` указывает, что в docker-compose.yml используется неверный ключ `port`.
   Правильное имя ключа для указания портов — **`ports`** (во множественном числе).
   ```bash
   [root@stapp02 docker]# cat docker-compose.yml 
   name: myapp

   services:
       web:
           image: ./app
           container_name: python
           port:
               - "5000:5000"
           volumes:
               - ./app:/code
           depends_on:
               - redis
       redis_app:
           image: redis
           container_name: redis
   ```
5. ```bash
   [root@stapp02 docker]# vi docker-compose.yml
   ```
   
   a) Указана версия файла Compose : `version: "3.9"`.  
   b) Если вы хотите использовать локальный образ (например, из папки `./app`), добавьте ключ `build`. Если образ уже существует, оставьте `image`.  
   c) `port` — неверный ключ, его нужно заменить на `ports`.  
   d) Использование правильного имени сервиса `redis`. В `depends_on:` указан сервис `redis`, но в файле он назван `redis_app`. Это вызывает путаницу. Имя сервиса должно совпадать.  

   ```bash
   [root@stapp02 docker]# cat docker-compose.yml 
   name: myapp
   version: 3.9
   
   services:
       web:
           build: ./app
           image: myapp:latest
           container_name: python
           ports:
               - "5000:5000"
           volumes:
               - ./app:/code
           depends_on:
               - redis_app
       redis_app:
           image: redis
           container_name: redis
   ```
7. ```bash
   [root@stapp02 docker]# docker compose up -d
   yaml: line 6: found character that cannot start any token
   ```
   
   Ошибка `yaml: line 6: found character that cannot start any token` указывает на синтаксическую ошибку в файле `docker-compose.yml`.
   Такая ошибка часто возникает из-за неправильных отступов, лишних символов или неправильной структуры YAML.
   Используйте встроенную проверку YAML, чтобы найти возможные ошибки:
   ```bash
   docker compose config
   ```
   Если файл корректен, команда отобразит его структуру. Если есть ошибки, они будут указаны.
   
   ```bash
   [root@stapp02 docker]# docker compose up -d
   validating /opt/docker/docker-compose.yml: version must be a string
   ```
   Ошибка `version must be a string` указывает, что в файле `docker-compose.yml` указано значение версии, которое не распознано как строка.
   Обычно это происходит, если вы используете числовое значение для версии, например, version: 3.9, вместо строки "3.9".
   Пояснение: кавычки обязательны, чтобы YAML правильно интерпретировал это значение как строку. Без кавычек версия может быть воспринята как число, что недопустимо.
   В новых версиях Docker Compose (2.x и выше) можно вообще убрать строку `version`, так как она устарела.
   
8. ```bash
   [root@stapp02 docker]# docker compose up -d
   WARN[0000] /opt/docker/docker-compose.yml: the attribute version is obsolete, it will be ignored, please remove it to avoid potential confusion 
   ```
   Сообщение `the attribute 'version' is obsolete, it will be ignored, please remove it to avoid potential confusion` означает,
   что вы используете новую версию Docker Compose (начиная с версии 2.0), которая больше не требует указания версии (`version`) в файле `docker-compose.yml`.
   Это сообщение — просто предупреждение и не мешает работе, но для чистоты можно удалить строку `version`. (Новые версии Docker Compose игнорируют эту строку.)

9. ```dockerfile
   ~~~
          web:
            build: ./app
            image: myapp:latest
            container_name: python
   ~~~
   ```
   Ошибка `pull access denied for myapp, repository does not exist or may require 'docker login'` указывает,
    что Docker пытается загрузить образ с именем `myapp` из Docker Hub, но такого образа там нет.
    Эта проблема возникает, если вы указали `image: myapp:latest`, но образ не существует локально или в удаленном репозитории.
    Если вы хотите собрать образ из исходного кода (например, из директории `./app`), нужно использовать ключ `build` в `docker-compose.yml`.

10. Итоговый docker-compose.yml:  
    ```bash
    [root@stapp02 docker]# cat /opt/docker/docker-compose.yml 
    name: myapp
    
    services:
        web:
            build: ./app
            container_name: python
            ports:
                - "5000:5000"
            volumes:
                - ./app:/code
            depends_on:
                - redis_app
        redis_app:
            image: redis
            container_name: redis
    ```
11. ```bash
    [root@stapp02 docker]# docker compose up -d --build
    [+] Building 0.1s (10/10) FINISHED                                                                          docker:default
     => [web internal] load build definition from Dockerfile                                                              0.0s
     => => transferring dockerfile: 151B                                                                                  0.0s
     => WARN: JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signa  0.0s
     => [web internal] load metadata for docker.io/library/python:3.13.0b1-slim-bullseye                                  0.0s
     => [web internal] load .dockerignore                                                                                 0.0s
     => => transferring context: 2B                                                                                       0.0s
     => [web internal] load build context                                                                                 0.0s
     => => transferring context: 92B                                                                                      0.0s
     => [web 1/4] FROM docker.io/library/python:3.13.0b1-slim-bullseye                                                    0.0s
     => CACHED [web 2/4] ADD . /code                                                                                      0.0s
     => CACHED [web 3/4] WORKDIR /code                                                                                    0.0s
     => CACHED [web 4/4] RUN pip install -r requirements.txt                                                              0.0s
     => [web] exporting to image                                                                                          0.0s
     => => exporting layers                                                                                               0.0s
     => => writing image sha256:89ea09f48f24df81e8171663416b051fb1dba7c99f34be65b4782e7f45a1ef6a                          0.0s
     => => naming to docker.io/library/myapp-web                                                                          0.0s
     => [web] resolving provenance for metadata file                                                                      0.0s
    [+] Running 2/2
     ✔ Container redis   Running                                                                                          0.0s 
     ✔ Container python  Started
    ```
12. Предупреждение `JSON arguments recommended for CMD to prevent unintended behavior related to OS signals` связано с использованием строкового синтаксиса команды `CMD` в `Dockerfile`.
    Docker рекомендует использовать JSON-формат, так как он более безопасен и обеспечивает правильную обработку аргументов и сигналов операционной системы.
    Если в вашем Dockerfile команда CMD написана как строка:  
    ```dockerfile
    CMD python app.py
    ```
    Docker интерпретирует её как запуск через оболочку (sh -c).
    Это может привести к непредвиденному поведению, особенно при передаче сигналов от хостовой машины в контейнер,
    например при завершении работы контейнера.
    Используйте JSON-формат для определения CMD.
    Он указывает команду и её аргументы как массив:  
    ```dockerfile
    CMD ["python", "app.py"]
    ```
    Как это работает:  
       a) **Строковый синтаксис:** интерпретируется как `sh -c "python app.py"`.  
          - Уязвим для неправильной обработки аргументов.  
          - Может игнорировать сигналы ОС.  
       b) **JSON-синтаксис:** запускает команду без оболочки, что:  
          - Исключает ошибки интерпретации.  
          - Обеспечивает правильную обработку сигналов.  
13. ```bash
    [root@stapp02 docker]# docker images
    REPOSITORY   TAG                      IMAGE ID       CREATED          SIZE
    myapp-web    latest                   89ea09f48f24   12 minutes ago   144MB
    myapp        latest                   89ea09f48f24   12 minutes ago   144MB
    redis        latest                   6c199afc1dae   8 weeks ago      117MB
    python       3.13.0b1-slim-bullseye   55c499a8da5e   6 months ago     126MB
    [root@stapp02 docker]# docker ps -a
    CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                    NAMES
    3f98b88de792   myapp-web   "/bin/sh -c 'python …"   4 minutes ago    Up 3 minutes    0.0.0.0:5000->5000/tcp   python
    c2f14dca4c93   redis       "docker-entrypoint.s…"   13 minutes ago   Up 12 minutes   6379/tcp                 redis
    ```
------------------------------



