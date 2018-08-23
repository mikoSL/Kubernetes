# Certified Kubernetes Administrator Course Studying Note

## Set up cluster,
1. create 6 nodes and select one as master node then config master mode as following
```
$ sudo kubeadm init --pod-network-cdir=10.244.0.0/16

```
2. setup cluster
```
$ mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

```
3. put up several Kubernetes servers in turn provide networking between pods
```
$ sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml

```
4. check pods
```
$ kubectl get pods --all-namespaces

```
4. copy the following, login to one node to join the node to the cluster
```
$ sudo kubeadm join 172.31.41.170:6443 --token xxxxxx  --discovery-token-ca-cert-hash sha256:4xxxxxxxxxxxxxxxxx

```

5. Go to the master node and check if the new node has been joined to cluster by following:

```
$ kubectl get nodes

```
