# Cluster Maintenance

## Upgrading
### 1 Upgrading Kubernetes Component

```
# check version
$ kubectl get nodes

# upgrade kubeadm
$ sudo apt upgrade kubeadm
$ kubeadm version

# first, update plan
$ sudo kubeadm upgrade plan

# update Components in Kubernetes
$ sudo kubeadm upgrade apply vx.x.x
```
* Upgrading Kubernetes Component

```
# check the pods status in each node
$ kubectl get pods -o wide

# Evict the running pod from the node which should be upgraded.
$ kubectl drain node-name --ignore-deamonsets

$ ssh node-name
$ sudo apt update
$ sudo apt upgrade kubelet

#check the current status
$ systemctl status kubelet
$ kubectl get nodes
$ kubectl uncordon pod-name

```

### 2 Upgrading the underlying OS
```
#drain the nodes of active pods
$ kubectl get nodes
$ kubectl drain node-name --ignore-deamonsets

#take one node out of serves
$ kubectl delete node node-name
```
* add one new nodes
```
# check the token
$ sudo kubeadm token list

#if token expires, create a new token, then add it to the list, so that you can add new node to cluster.
$ sudo kubeadm token generate
$ sudo kubeadm token create token-created-above  --ttl 3h --print-join-command

# copy the command
$ sudo join-command-created-aboved

# check the nodes
$ kubectl get nodes
```
