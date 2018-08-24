# Replication controller, replica sets and deployment

## 1. Write a YAML file that will create a Replication Controller that will maintain three copies of an nginx container.  Execute your YAML and make sure it works.
* A Replication Controller ensures that a specified number of pod replicas are running at any one time. In other words, a Replication Controller makes sure that a pod or a homogeneous set of pods is always up and available.

```
apiVersion: v1
kind: ReplicationController
metadata:
 name: nginx
spec:
 replicas: 3
 selector:
  app: nginx
 template:
  metadata:
   name: nginx
   labels:
    app: nginx
  spec:
   containers:
   - name: nginx
     image: nginx
     ports:
     - containerPort: 80


```
## 2. Write the YAML that will maintain three copies of an nginx container using a Replica Set.  Test it to be sure it works, then delete it.
* A Replica Set is a more advanced version of a Replication Controller that is used when **more low-level control** is needed.  While these are commonly managed with deployments in modern K8s, it's good to have experience with them.
```
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
 name: nginx
 labels:
  app: nginx
spec:
 replicas: 3
 selector:
  matchLabels:
   app: nginx
 template:
  metadata:
   labels:
    app: nginx
  spec:
   containers:
   - name: nginx
     image: nginx
     ports:
     - containerPort: 80
```

## 3. Write the YAML for a deployment that will maintain three copies of an nginx container.  Test it to be sure it works, then delete it.
```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
 name: nginx-deployment
 labels:
  app: nginx
spec:
 replicas: 3
 selector:
  matchLabels:
   app: nginx
 template:
  metadata:
   labels:
    app: nginx
  spec:
   containers:
   - name: nginx
     image: nginx
     ports:
     - containerPort: 80
```
