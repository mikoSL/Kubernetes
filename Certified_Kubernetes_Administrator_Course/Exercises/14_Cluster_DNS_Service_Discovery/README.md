# Cluster DNS Service Discovery

## In a Kubernetes cluster, services discover one another through the Cluster DNS.  Names of services resolve to their ClusterIP, allowing application developers to **only know the name of the service deployed** in the cluster and **not** have to figure out how to **get all the right IP addresses** into the right containers at deploy time.

## Create yaml file bit-of-nothing
```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: bit-of-nothing
spec:
  selector:
    matchLabels:
      app: pause
  replicas: 2
  template:
    metadata:
      labels:
        app: pause
    spec:
      containers:
      - name: bitty
        image: k8s.gcr.io/pause:2.0
```

## 1. Run the deployment and verify that the pods have started.
```
$ kubectl create -f bit-of-nothing.yaml
$ kubectl get pods
```
## 2. Start a busybox pod you can use to check DNS resolution.
```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
spec:
  containers:
  - name: busybox
    image: busybox
    command:
      - sleep
      - "3600"
    imagePullPolicy: IfNotPresent
  restartPolicy: Always
```

## 3. Check to see whether or not bit-of-nothing is currently being resolved in the cluster via your busybox container.
```
$ kubectl create -f busybox.yaml
```
## 4. Expose the bit-of-nothing deployment as a ClusterIP service.
```
$ kubectl expose deployment bit-of-nothing --type=ClusterIP --port 80
```
## 5. Verify that "bit-of-nothing" is now being resolved to an IP address in your cluster.
```
$ kubectl exec -it busybox -- nslookup bit-of-nothing
```
