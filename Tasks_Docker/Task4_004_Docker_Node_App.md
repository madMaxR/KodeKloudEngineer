
------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-17 19:19:00  
Finished: &nbsp;&nbsp;2024-12-17 19:56:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 4_4: Docker Node App

## Requirements

There is a requirement to Dockerize a Node app and to deploy the same on `App Server 2`.  
Under `/node_app` directory on `App Server 2`, we have already placed a `package.json` file that   
describes the app dependencies and `server.js` file that defines a web app framework.  

Create a `Dockerfile` (name is case sensitive) under `/node_app` directory:

   Use any `node` image as the base image.  
   Install the dependencies using `package.json` file.  
   Use `server.js` in the `CMD`.  
   Expose port `8083`.  

The build image should be named as `nautilus/node-web-app`.

Now run a container named `nodeapp_nautilus` using this image.

Map the container port `8083` with the host port `8098`.

Once deployed, you can test the app using a `curl` command on `App Server 2`:
`curl http://localhost:8098`

------------------------------

## Steps

1. ```bash
   thor@jumphost ~$ ssh steve@stapp02
   ```
2. ```bash
   [steve@stapp02 node_app]$ sudo su -
   ```
3. ```bash
   [root@stapp02 ~]# cd /node_app/
   ```
4. ```bash
   [root@stapp02 node_app]# touch Dockerfile  
   [root@stapp02 node_app]# vi Dockerfile   
   [root@stapp02 node_app]# cat Dockerfile
   ```
   ```dockerfile
   FROM node:latest
   WORKDIR /node_app
   COPY package.json ./
   RUN npm install
   COPY . .
   EXPOSE 8083
   CMD ["node", "server.js"]
   ```
5. ```bash
   [root@stapp02 node_app]# docker build -t nautilus/node-web-app /node_app
   [+] Building 61.0s (2/2) FINISHED                                                                           docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 156B                                                                                  0.0s
    => ERROR [internal] load metadata for docker.io/library/node:latest                                                 60.9s
   ------
    > [internal] load metadata for docker.io/library/node:latest:
   ------
   Dockerfile:1
   --------------------
      1 | >>> FROM node:latest
      2 |     WORKDIR /node_app
      3 |     COPY package.json ./
   --------------------
   ERROR: failed to solve: DeadlineExceeded: DeadlineExceeded: DeadlineExceeded: node:latest:  
   failed to resolve source metadata for docker.io/library/node:latest: failed to copy:  
   httpReadSeeker: failed open: failed to do request:  
   Get "https://docker-registry-mirror.kodekloud.com/v2/library/node/manifests/sha256:  
   0b50ca11d81b5ed2622ff8770f040cdd4bd93a2561208c01c0c5db98bd65d551?ns=docker.io":  
   dial tcp 10.0.0.6:443: i/o timeout
   ```
6. Pull the image manually or try a different base image:
   ```bash
   docker pull node:23
   ```
   or
   ```bash
   FROM node:23
   WORKDIR /node_app
   COPY package.json ./
   RUN npm install
   COPY . .
   EXPOSE 8083
   CMD ["node", "server.js"]
   ```
7. ```bash
   [root@stapp02 node_app]# docker build -t nautilus/node-web-app /node_app
   [+] Building 402.3s (10/10) FINISHED                                                                        docker:default
    => [internal] load build definition from Dockerfile                                                                  0.0s
    => => transferring dockerfile: 152B                                                                                  0.0s
    => [internal] load metadata for docker.io/library/node:23                                                          120.9s
    => [internal] load .dockerignore                                                                                     0.0s
    => => transferring context: 2B                                                                                       0.0s
    => [1/5] FROM docker.io/library/node:23@sha256:0b50ca11d81b5ed2622ff8770f040cdd4bd93a2561208c01c0c5db98bd65d551    176.1s
    => => resolve docker.io/library/node:23@sha256:0b50ca11d81b5ed2622ff8770f040cdd4bd93a2561208c01c0c5db98bd65d551      0.0s
    => => sha256:0b50ca11d81b5ed2622ff8770f040cdd4bd93a2561208c01c0c5db98bd65d551 6.41kB / 6.41kB                        0.0s
    => => sha256:a81372dcb7a0d4a183f453f04a8eba1f47ff44089294dcf73e5e368ec56d58d4 2.49kB / 2.49kB                        0.0s
    => => sha256:551df7f94f9c131f2fec0e8063142411365f0a1c88b935b9fac22be91af227e0 64.39MB / 64.39MB                     31.7s
    => => sha256:2a4173b745f902090dff98f3fe1e58b351c36b2d305aa0c0c98b910fdbfcad7e 6.39kB / 6.39kB                        0.0s
    => => sha256:fdf894e782a221820acf469d425b802be26aedb5e5d26ea80a650ff6a974d488 48.50MB / 48.50MB                     31.2s
    => => sha256:5bd71677db44bb63b94de61b6f1f95d5540b4ba2d6a8a6bc4d19f422b25e0c2b 23.87MB / 23.87MB                     30.7s
    => => sha256:ce82e98d553dd62ca6a12bebfe83992ae9f9ae2748275e74b66a68cc094f868b 211.31MB / 211.31MB                   64.1s
    => => sha256:2116f34ae2dd740f78fbbdac14f1b565757125065fbeca415c99d3e095443b41 3.33kB / 3.33kB                       61.3s
    => => extracting sha256:fdf894e782a221820acf469d425b802be26aedb5e5d26ea80a650ff6a974d488                             6.0s
    => => sha256:a05238f4980b8654a5fa5aee4fa2ff7dd9e908947e7c78b3dddf1b5b369716bf 56.89MB / 56.89MB                     63.0s
    => => extracting sha256:5bd71677db44bb63b94de61b6f1f95d5540b4ba2d6a8a6bc4d19f422b25e0c2b                             1.7s
    => => extracting sha256:551df7f94f9c131f2fec0e8063142411365f0a1c88b935b9fac22be91af227e0                             8.0s
    => => sha256:9624856701e2eae966a1311fa330f715a23f9d907bb1c9c1abf164680725f1d6 1.25MB / 1.25MB                       91.8s
    => => sha256:7d7b66e48eb6b4e20f97e0edc4cb076d4b30c079163b0c470ee30ca52a7887bb 446B / 446B                           93.1s
    => => extracting sha256:ce82e98d553dd62ca6a12bebfe83992ae9f9ae2748275e74b66a68cc094f868b                            20.9s
    => => extracting sha256:2116f34ae2dd740f78fbbdac14f1b565757125065fbeca415c99d3e095443b41                            16.2s
    => => extracting sha256:a05238f4980b8654a5fa5aee4fa2ff7dd9e908947e7c78b3dddf1b5b369716bf                             7.6s
    => => extracting sha256:9624856701e2eae966a1311fa330f715a23f9d907bb1c9c1abf164680725f1d6                            21.6s
    => => extracting sha256:7d7b66e48eb6b4e20f97e0edc4cb076d4b30c079163b0c470ee30ca52a7887bb                            21.9s
    => [internal] load build context                                                                                     0.0s
    => => transferring context: 863B                                                                                     0.0s
    => [2/5] WORKDIR /node_app                                                                                          26.9s
    => [3/5] COPY package.json ./                                                                                       21.6s
    => [4/5] RUN npm install                                                                                            24.7s
    => [5/5] COPY . .                                                                                                   21.9s 
    => exporting to image                                                                                               10.0s 
    => => exporting layers                                                                                               9.9s 
    => => writing image sha256:4596f0183b74ad570f7ef78aa01a183b3203cdddb815296f5e5d2f58b4388fc6                          0.0s 
    => => naming to docker.io/nautilus/node-web-app 
   ```
8. ```bash
   [root@stapp02 node_app]# docker run -d --name nodeapp_nautilus -p 8098:8083 nautilus/node-web-app
   5c5792575303d974ac207bb7c385650eeee02bd20c3ba20a1235ec5a4aebbb9f
   ```
9. ```bash
   [root@stapp02 node_app]# curl http://localhost:8098
   Welcome to xFusionCorp Industries!
   ```
10. Tests:
    ```bash
    [root@stapp02 node_app]# docker images
    REPOSITORY              TAG       IMAGE ID       CREATED         SIZE
    nautilus/node-web-app   latest    4596f0183b74   5 minutes ago   1.13GB
    [root@stapp02 node_app]# docker ps -a
    CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS              PORTS                    NAMES
    5c5792575303   nautilus/node-web-app   "docker-entrypoint.s…"   2 minutes ago   Up About a minute   0.0.0.0:8098->8083/tcp   nodeapp_nautilus
    ```
[//]: # ( You have successfully completed the challenge.Results have been saved. Ref ID:6481fbd05aecb08e6658f91e )
------------------------------



