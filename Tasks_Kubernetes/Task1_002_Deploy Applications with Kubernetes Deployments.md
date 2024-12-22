------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-22 20:35:00
Finished: &nbsp;&nbsp;2024-12-22 20:40:00

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1_2: Deploy Applications with Kubernetes Deployments

## Requirements

The Nautilus DevOps team is delving into Kubernetes for app management.
One team member needs to create a deployment following these details:

   - Create a deployment named `nginx` to deploy the application `nginx` using the image `nginx:latest` (ensure to specify the tag)

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

------------------------------

## Steps

```bash
thor@jumphost ~$ kubectl create deployment nginx --image nginx:latest
deployment.apps/nginx created
thor@jumphost ~$ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-7bf8c77b5b-zlpbw   1/1     Running   0          31s
thor@jumphost ~$ kubectl get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           42s
thor@jumphost ~$ kubectl describe deployment/nginx
Name:                   nginx
Namespace:              default
CreationTimestamp:      Sun, 22 Dec 2024 17:38:54 +0000
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:         nginx:latest
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-7bf8c77b5b (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  52s   deployment-controller  Scaled up replica set nginx-7bf8c77b5b to 1
```
