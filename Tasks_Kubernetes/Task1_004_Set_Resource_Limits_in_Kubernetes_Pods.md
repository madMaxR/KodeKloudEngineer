------------------------------

Start: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2024-12-26 23:14:00  
Finished: &nbsp;&nbsp;2024-12-26 23:19:00  

------------------------------

- [Requirements](#requirements)
- [Steps](#steps)

------------------------------

# TASK 1_4: _Set Resource Limits in Kubernetes Pods

## Requirements

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints.
To address this, they plan to set limits on resource utilization. Here are the details:

Create a pod named `httpd-pod` with a container named `httpd-container`. 
Use the `httpd` image with the `latest` tag (specify as `httpd:latest`). 
Set the following resource limits:

*Requests*: Memory: **15Mi**, CPU: **100m**

*Limits*: Memory: **20Mi**, CPU: **100m**

***Note***: The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

------------------------------

## Steps

1)
```bash
thor@jumphost ~$ touch pod.yml
thor@jumphost ~$ vi pod.yml 
thor@jumphost ~$ cat pod.yml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```
2)
```
thor@jumphost ~$ kubectl apply -f pod.yml
pod/httpd-pod created
thor@jumphost ~$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          11s
thor@jumphost ~$ kubectl describe pods httpd-pod | grep Containers: -A 20
Containers:
  httpd-container:
    Container ID:   containerd://d28d5a009216e81c8b1c95852049d7c070a9063c068758fc8170f0035faa9e2d
    Image:          httpd:latest
    Image ID:       docker.io/library/httpd@sha256:72f6e24600718dddef131de7cb5b31496b05c5af41e9db8707df371859a350bb
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 25 Dec 2024 21:10:55 +0000
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  20Mi
    Requests:
      cpu:        100m
      memory:     15Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-zw6t2 (ro)
Conditions:
```
