------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-19 21:14:00  
Finished: &nbsp;&nbsp;2024-12-19 21:22:00  

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1_3: Setup Kubernetes Namespaces and PODs

## Requirements

The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform.
The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc.
Based on the current requirements, the team has shared some details as below:

Create a namespace named `dev` and deploy a POD within it.
Name the pod `dev-nginx-pod` and use the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.

**Note:** The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

------------------------------

## Steps

1) 
```bash
thor@jumphost ~$ touch nginx.yml
thor@jumphost ~$ vi nginx.yml 
thor@jumphost ~$ cat nginx.yml 
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  namespace: dev
spec:
  containers:
    - name: nginx
      image: nginx:latest
thor@jumphost ~$ kubectl apply -f .
namespace/dev created
pod/dev-nginx-pod created
```
```bash
thor@jumphost ~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   25m
dev                  Active   41s
kube-node-lease      Active   25m
kube-public          Active   25m
kube-system          Active   25m
local-path-storage   Active   25m
thor@jumphost ~$ kubectl get all -n dev
NAME                READY   STATUS    RESTARTS   AGE
pod/dev-nginx-pod   1/1     Running   0          100s
thor@jumphost ~$ kubectl describe -n dev pod dev-nginx-pod | grep Image:
    Image:          nginx:latest
```
