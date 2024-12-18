------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-18 21:01:00  
Finished: &nbsp;&nbsp;2024-12-18 21:36:00  

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4_5: Docker Python App

## Requirements

A python app needed to be Dockerized, and then it needs to be deployed on `App Server 1`. 
We have already copied a `requirements.txt` file (having the app dependencies) under `/python_app/src/` directory on `App Server 1`. 
Further complete this task as per details mentioned below:

1. Create a `Dockerfile` under `/python_app` directory:

   - Use any `python` image as the base image.  
     
   - Install the dependencies using `requirements.txt` file.  
     
   - Expose the port `5002`.  
     
   - Run the `server.py` script using `CMD`.  
     

2. Build an image named `nautilus/python-app` using this Dockerfile.


3. Once image is built, create a container named `pythonapp_nautilus`:

   - Map port `5002` of the container to the host port `8096`.

4. Once deployed, you can test the app using `curl` command on `App Server 1`:  
```bash
curl http://localhost:8096/
```

------------------------------

## Steps

1) ssh to stapp01 (172.16.238.10)

2) ```bash
   [tony@stapp01 ~]$ sudo su -
   ```
3) ```bash
   [root@stapp01 ~]# cd /python_app/
   [root@stapp01 python_app]# touch Dockerfile
   [root@stapp01 python_app]# ls
   Dockerfile  src
   [root@stapp01 python_app]# ls src/
   requirements.txt  server.py
   [root@stapp01 python_app]# cat src/requirements.txt 
   flask
   [root@stapp01 python_app]# cat src/server.py 
   from flask import Flask
   
   # the all-important app variable:
   app = Flask(__name__)
   
   @app.route("/")
   def hello():
       return "Welcome to xFusionCorp Industries!"
   
   if __name__ == "__main__":
           app.config['TEMPLATES_AUTO_RELOAD'] = True
   ```
4) ```bash
   [root@stapp01 python_app]# vi Dockerfile
   ```
   ```dockerfile
   [root@stapp01 python_app]# cat Dockerfile 
   FROM python:3.13.1-slim
   WORKDIR /app
   COPY src /app
   RUN pip install --no-cache-dir -r requirements.txt
   EXPOSE 5002
   CMD ["python", "server.py"]
   ```
5) ```bash
   [root@stapp01 python_app]# docker pull python:3.13.1-slim
   3.13.1-slim: Pulling from library/python
   bc0965b23a04: Pull complete 
   b2274c5340d4: Pull complete 
   31679bb6d802: Pull complete 
   5b0f4af99364: Pull complete 
   Digest: sha256:f41a75c9cee9391c09e0139f7b49d4b1fbb119944ec740ecce4040626dc07bed
   Status: Downloaded newer image for python:3.13.1-slim
   docker.io/library/python:3.13.1-slim
   [root@stapp01 python_app]# docker build -t nautilus/python-app /python_app
   [+] Building 14.5s (9/9) FINISHED                                                                           docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 181B                                                                                  0.0s
    => [internal] load metadata for docker.io/library/python:3.13.1-slim                                                 0.0s
    => [internal] load .dockerignore                                                                                     0.1s
    => => transferring context: 2B                                                                                       0.0s
    => CACHED [1/4] FROM docker.io/library/python:3.13.1-slim                                                            0.0s
    => [internal] load build context                                                                                     0.0s
    => => transferring context: 97B                                                                                      0.0s
    => [2/4] WORKDIR /app                                                                                                5.5s
    => [3/4] COPY src /app                                                                                               1.4s
    => [4/4] RUN pip install --no-cache-dir -r requirements.txt                                                          5.9s
    => exporting to image                                                                                                1.4s 
    => => exporting layers                                                                                               1.3s 
    => => writing image sha256:272dd2a23b36df50bf7008a0641139c65631f72b5c2c02e2b5a26e2f96a3c4d4                          0.0s 
    => => naming to docker.io/nautilus/python-app
   ```
6) ```bash
   [root@stapp01 python_app]# docker run -d --name pythonapp_nautilus -p 8096:5002 nautilus/python-app
   6ca2da1ef48251eba23b6338d42f7ae33ad14b49bc6ad58a87573a6fed1efa9d
   ```
7) ```bash
   [root@stapp01 python_app]# curl http://localhost:8096/
   Welcome to xFusionCorp Industries!
   ```
8) ```bash
   [root@stapp01 python_app]# docker logs pythonapp_nautilus
    * Serving Flask app 'server'
    * Debug mode: on
   WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:5002
    * Running on http://172.12.0.2:5002
   Press CTRL+C to quit
    * Restarting with stat
    * Debugger is active!
    * Debugger PIN: 141-808-125
   172.12.0.1 - - [18/Dec/2024 18:29:07] "GET / HTTP/1.1" 200 -
   ```
9) ```bash
   [root@stapp01 python_app]# docker ps -a
   CONTAINER ID   IMAGE                 COMMAND              CREATED         STATUS         PORTS                    NAMES
   6ca2da1ef482   nautilus/python-app   "python server.py"   8 minutes ago   Up 8 minutes   0.0.0.0:8096->5002/tcp   pythonapp_nautilus
   [root@stapp01 python_app]# docker images
   REPOSITORY            TAG           IMAGE ID       CREATED         SIZE
   nautilus/python-app   latest        272dd2a23b36   9 minutes ago   133MB
   python                3.13.1-slim   72c1629a0805   2 weeks ago     120MB
   ```
[//]: # ( You have successfully completed the challenge.Results have been saved. Ref ID:6481fc1b5aecb08e6658f91f )

------------------------------

