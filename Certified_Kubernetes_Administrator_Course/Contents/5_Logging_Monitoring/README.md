# Logging & Monitoring

## 1. Monitoring cluster component and application(**Heapster**)
* **Heapster** cluster-wide aggregator of monitoring and event data. It runs as a pod inside cluster. It discovers all nodes in the cluster, then query usage information from the node. Heapters fetches data from **cAdvisor**. Heapster is setup to use this storage backend by defalut on most Kubernetes clusters.
* **cAdvisor** is an open source container resource usage and performance analysis agent. Normally on port 4194. Usage statistics are obtained from cAdvisor.
* Expose aggregated pod resource usage statistics via a REST API.
* **Kubelet** aces as a brideg between Kubernetes master and nodes.
* **Grafana with InfluxDB** run in Pods.The pod exposes itself as a Kubernetes **service** which is how **Heapster then discovers** it.
* Grafana container serves Grafana's UI which provides a dashboard.
* The default dashboard for Kubernetes contains an example dashboard that monitors resoruce usage of the cluster and the pods inside of it.
* **Google Cloud Monitoring** is a hosted monitoring service. **Heapster** can be set up to automatically push all collected metrics to Google cloud monitoring.

## 2. Managing Logs
* Logs inside pods
```
$ kubectl get pods
$ kubectl logs pods-name
```
* Logs of containers
```
$ kubectl get pods --all-namespaces
$ cd /var/log/contains
```
