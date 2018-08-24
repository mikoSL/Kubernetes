# Application Lifecycle Management

## 1. Deployment, rolling updates and rollback
* tip: how to get yaml file out

```
$ kubectl get deployment deployments-name -o yaml

```

* rollout with command
```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.8
$ kubectl roolout status deployment/nginx-deployment
$ kubectl describe deployment nginx-deployment

```

* rollout with yaml
```
$ vi nginx-deployment.yaml

// edit yaml file, save and exist

$ kubectl apply -f nginx-deployment.yaml
$ kubectl roolout status deployment/nginx-deployment
$ kubectl describe deployment nginx-deployment

```

* check roolout history, find error if there is, then undo
```
$ kubectrl rollout history deployment/nginx-deployment --revision=3
$ kubectrl rollout history deployment/nginx-deployment --revision=2
$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

## 2. Configure applications(**ConfiMap**: great way to **decouple configuration of a particular container/application from yaml we use to instantiated**)

* create configMap
```
$ kubectrl create configMap my-map --from-literal=school=linuxAcademy

$ kubectl get configMap

$ kubectl describe  configMap

$ kubectl get configMap my-map -o yaml
```
* Use the configMap (using pod-config.yaml)
```
$ kubectrl create -f pod-config.yaml

$ kubectl get pods --show-all

$ kubectl logs config-test-pod
```

## 3. Scaling applications(scale up to 3 replicas and scale down)
```
$ kubectrl scale deployment/nginx-deployment --replicas=3
```
```
$ kubectrl scale deployment/nginx-deployment --replicas=1
```
## 4. Self-healing application
* Kubernets will restart **pods** when it is deleted.
* When node server is stopped. **pods** which running in it will not be **evicted until 5 mins(300s)** if there is problem. Then **new pod** will be automatically **created**.
