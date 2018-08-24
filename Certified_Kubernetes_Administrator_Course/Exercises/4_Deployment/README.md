# Deployment

## Create the deployment

```
$ kubectl create -f nginx-deployment.yaml

```
## Which nodes are the pods running on?
```
kubectl describe deployment nginx-deployment

```
or
```
kubectl get pods -l app=nginx -o wide

```
or
```
kubectl get pods name-of-pods -o wide

```
## Update the deployment to use the 1.8 version of the nginx container and roll it out.

```
$ vi nginx-deployment.yaml

// edit it, save it

$ kubectl apply -f nginx-deployment.yaml

```

or
```
$ kubectl set image deployment nginx-deployment nginx=nginx:1.8

```
## Update the deployment to use the 1.9 version of the nginx container and roll it out.

## Roll back the deployment to the 1.8 version of the container.
```
$ kubectl rollout undo deployment nginx-deployment

```
or go to specific version
```
$ kubectl rollout history deployment nginx-deployment
$ kubectl rollout undo deployment nginx-deployment --to-revision x

```

## Delete the Deployment
```
$ kubectl delete deployment nginx-deployment 

```
