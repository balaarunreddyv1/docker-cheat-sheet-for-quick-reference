**kubernetes is a popular container orchestrator**

**conatiner orchestrator** =  make many servers act like one -> takes all the containers, takes a series of servers or nodes and decides how to run, manage them.

kubernetes runs on top of docker as a set of API's in containers.
kubernetes provides API/CLI to manage containers across servers.



- **Many app** run in containers -> wrapped together we call them as **pods**.
- **Many pods** are managed by **Replica Set**. (****Replica Set**** can create many duplicate pods).
- **Replica Set** is managed by **Deployment yaml context**.
- **ALL COME TOGETHER IN ONE YAML FILE**.


https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-arun
  labels:
    app: nginx-arun-pod
spec:
  containers:
    - name: nginx-arun-container
      image: nginx:latest
      imagePullPolicy: IfNotPresent
      resources:
        limits:
          cpu: "500m"
          memory: "128Mi"
      ports:
        - containerPort: 80

  restartPolicy: Always
```
**
Create POD config file:**
- ` kubectl create -f<<path/filename.extension>>`

**Get All Pods Info**
- `kubectl get pods`

**Apply new changes to exisiting pods**
- ` kubectl apply -f <<path/filename.extension>>`

**delete pods**
- `kubectl delete pods <<POD_NAME>>`


**ReplicaSet syntax**
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name:
```

**Replica set**
- `kubectl get replicasets`

**watch pods  live**
- `watch kubectl get pods`

**delete replica set**
- `kubectl delete replicasets <<ReplicaSet Name>>`

**inspect pod**
- `kubectl describe pod <<Name>>`

**interact with pod**
- `kubectl exec it <<Name>> -c server -- /bin/bash`

**delete deployment**
- `kubectl delete deployment <<name>>`

**Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.**

`minikude delete` -> if any issues, delete the vm that minikude sets up

enable ingress addons -> `minikude addons enable ingress` 

https://kubernetes.io/docs/concepts/services-networking/ingress/#:~:text=What%20is%20Ingress%3F,Figure.
