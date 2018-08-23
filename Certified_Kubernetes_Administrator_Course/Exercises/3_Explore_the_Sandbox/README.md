# Explore the sandbox

## Examine the current status of your cluster.

### 1. Are all the nodes ready?
```
$ kubectl get nodes

```
### 2. Are there any pods running on node 2 of your cluster? How can you tell?
```
kubectl describe node node-name

```
or
```
kubectl get pods --all-namespaces -o wide

```
### 3. Is the master node low on memory currently? Check **DiskPressure** and **MemoryPressure** after running the following

```
$ kubectl describe nodes node-name

```
### 4. What pods are running in the kube-system namespace?
```
$ kubectl get pods -n kube-system

```
