# Networking

## 1. Node network configuration
* Inbound node port requirement
1. Master node:
 * TCP 6443 -- Kubernets API server
 * TCP 2376-2380 -- etcd server client API
 * TCP 10250 -- Kubelet API
 * TCP 10251 -- kube-scheduler
 * TCP 10255 -- Read-only Kubelet API
2. Worker node:
 * TCP 10250 -- Kubelet API
 * TCP 10255 -- Read-only Kubelet API
 * TCP 30000 - 32767 -- NodePort Service

## 2. Service Networking
```
$ kubectl expose deployment deployment-name --type="NodePort" --port 80
$ kubectl get services
$ curl localhost:32516
```
## 3. Ingress
* It is API object that manages external access to the services in a cluster, normally HTTP
* It can provide load balancing, SSL termination and name-based virtual hosting.
* An ingress is a collection of rules that allow inbound connections. Deploy an ingress controller in the master.
* IP allocated by the **ingress controller** to satisfy the ingress.
* Secure ingress by **specify secrets**(TLS private key, certificate), **port 443**, multiple hosts through **SNI TLS** extension, TLS extension must contain keys named **tls.crt** and **tls.key**
* **Ingress controller** is bootstrapped with a load balancing policy that it applies to all ingress objects(e.g. load balancing algorithm, backend weight scheme).
* **Health checks** are not exposed directly through the ingress.
* Update running ingress
```
$ kubectl get ing
$ kubectl edit ing test

// replace
$ kubectl replace -f xxx.yaml
```
* Other ways to expose a service that does not directly involve ingress resource:
1. Service.Type=LoadBalancer
2. Service.Type=NodePort
3. Port Proxy

## 4. Deploying a Load Balancer
```
apiVersion: v1
kind: Service
metadata:
 name: la-lb-services
spec:
 selector:
  app: la-lab
 ports:
 - protocal: TCP
   port: 80
   targetPort: 9376
 clusterIP: 10.0.171.223
 loadBalancerIP: 78.12.23.17
 type: LoadBalancer
```

## 5. Configure & Use Cluster DNS
```
$ kubectl get pods -n kube-system

$ kubectl get pods
$ kubectl get services

$ kubectl exec --it pod-name -- nslookup service-name  
```

## 6. Container Network Interface(CNI)
* CNI plugin
* All pods can communicate with all other pods. (IP address)
* Dynamic port allocation problem
* Kubernets ways:
 1. All containers can communicate with each other **without NAT**.
 2. All nodes can communicate with all containers(&vice versa) without NAT.
 3. IP of a container is the same regardless of which container views it.
 4. K8s applies IP addreses at the pod level.
 5. "**IP-per-Pod**" --containers in a pod share a single IP address, like processes in a VM.
