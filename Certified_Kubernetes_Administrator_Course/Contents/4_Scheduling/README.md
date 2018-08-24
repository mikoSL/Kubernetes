# Scheduling

## 1. Labels & Selectors
* Putting labels on objects in Kubernetes allow you to identify and select objects in as wide or granular style as you like.
* labels(key/value pair), label is not identical, pods can have many labels. label name example:
* Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
1. release
2. stable
3. canary
4. environment
5. dev
6. qa
7. production
8. frontend
9. partition
10. ....

* get particular pods add label flag **-l app=xx**
```
$ kubectl get pods -l app=nginx
```
* specify own labels
```
$ kubectl label pod pod-name test=sure --overwrite

$ kubectl describe pod -l test=sure
```
* replace previous labels or add new labels, add **tier=frontend** for all **app=nginx**
```
$ kubectl label pods -l app=nginx tier=frontend
```

* delete pods' labels, pods' label are deleted but pods will be spinned up automatically without any labels.
```
$ kubectl delete pods -l test=sure
```
## 2. DaemonSets(short **ds**)
* Use DeamsonSet for Pods that need to run **one per machine**, because they provide a **machine-specific system** service
```
$ kubectl get DaemonSets -n namespace-name
```
* get information
```
$ kubectl get DaemonSets DaemonSets-name -n namespace-name
```
## 3. Resource Limits & Pod Scheduling
* how to find **taints** and **tolerations**, they match with each other
```
$ kubectl describe node node-name
```
* untainted node so it can be **scheduled** without **tolerations**
```
$ kubectl taint nodes node-name node-role.Kubernetes.io/master-node "node-name" untained
```

* tained a node
```
$ kubectl tained nodes node-name node-role.Kubernetes.io=master:NoSchedule node "node-name" tained
```
## 4. Manually scheduling pods to particular node using labels
* set pod in node 3
1. put label on node 3
```
$ kubectl label node node3-name net=gigabit
$ kubectl describe node node3-name
```
2. pod should have selector on it to match node3 by adding **nodeSelector** in spec/template/spec in YAML file
```
nodeSelector:
 net=gigabit

$ kubectl create -f name-yaml.yaml
$ kubectl get nodes
$ kubectl describe pod 
```
