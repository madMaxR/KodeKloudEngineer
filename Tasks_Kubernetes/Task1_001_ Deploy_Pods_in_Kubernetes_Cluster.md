------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-19 20:15:00  
Finished: &nbsp;&nbsp;2024-12-19 20:32:00  

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1_1: Deploy Pods in Kubernetes Cluster

## Requirements

The Nautilus DevOps team is diving into Kubernetes for application management.
One team member has a task to create a pod according to the details below:

1. Create a pod named `pod-nginx` using the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.
2. Set the `app` label to `nginx_app`, and name the container as `nginx-container`.
**Note**: The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

------------------------------

## Steps

1) ```bash
   thor@jumphost ~$ touch pod.yml
   thor@jumphost ~$ vi pod.yml
   ```
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
       name: pod-nginx
       labels:
         app: nginx_app
   spec:
       containers:
       - name: nginx-container
         image: nginx:latest
   ```
2) ```bash
   thor@jumphost ~$ kubectl apply -f pod.yml
   pod/pod-nginx created
   ```
3) ```bash
   thor@jumphost ~$ kubectl get pods
   NAME        READY   STATUS    RESTARTS   AGE
   pod-nginx   1/1     Running   0          25s
   thor@jumphost ~$ kubectl logs -f pod-nginx
   /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
   /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
   /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
   10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
   10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
   /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
   /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
   /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
   /docker-entrypoint.sh: Configuration complete; ready for start up
   2024/12/19 17:21:56 [notice] 1#1: using the "epoll" event method
   2024/12/19 17:21:56 [notice] 1#1: nginx/1.27.3
   2024/12/19 17:21:56 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
   2024/12/19 17:21:56 [notice] 1#1: OS: Linux 5.4.0-1106-gcp
   2024/12/19 17:21:56 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
   2024/12/19 17:21:56 [notice] 1#1: start worker processes
   2024/12/19 17:21:56 [notice] 1#1: start worker process 79
   2024/12/19 17:21:56 [notice] 1#1: start worker process 80
   2024/12/19 17:21:56 [notice] 1#1: start worker process 81
   2024/12/19 17:21:56 [notice] 1#1: start worker process 82
   2024/12/19 17:21:56 [notice] 1#1: start worker process 83
   2024/12/19 17:21:56 [notice] 1#1: start worker process 84
   ```
[//]: # ( You have successfully completed the challenge.Results have been saved. Ref ID:64072040741b204d59fbe9d6 )

------------------------------

