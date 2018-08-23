# Installation, Configuration and Validation

## Design a Kubernetes cluster

## Hardware and underlying infrastructure

## Securing cluster communication(default is TLS)
### API server
* Anything that connects to API, including nodes, proxies, scheduler, volume plugins in addition to users, should be anthenticated.

* Kubernetes has **RBAC (Role-Based Access Control)** component.

### Kubelet
* Secure **kubelet** on **each** node. Kubelet exposes HTTPs endpoints which give access to both data and actions on the nodes. By default, these are open! To secure them, enable Kubelet authentication and authorization by starting an **-anonymous-auth=false** flag and assigning it to appropriate **x509 client certificate** in its configuration.

### Network
* Secure network (etcd should be strongly protected! Rotating credentials! )

## Make Kubernetes High-Available
* Create a reliable nodes(especially master node)
* Set up a redundant and reliable storage service with a multinode deployment of **etcd**.
* Start replicated and load balanced Kubernetes API servers.
* Set up a master-elected Kubernetes scheduler and controller manager deamons.

## Validate node and cluster
### end-to-end(e2e) test (e.g. kubetest suit in AWS)
### check the nodes status
