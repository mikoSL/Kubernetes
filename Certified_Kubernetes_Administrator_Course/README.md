# Certified Kubernetes Administrator Course Studying Note

## Set up cluster
1. master node
```
$ sudo kubeadm init --pod-network-cdir=10.244.0.0/16

```
2. setup cluster
```
$ mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

```
