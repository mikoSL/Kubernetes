# Scaling practices

## 1. Scale the deployment up to 4 pods without editing the YAML.

```
$ kubectl scale deployment nginx-deployment --replicas=4

```
## 2. Edit the YAML so that 3 pods are available and can apply the changes to the existing deployment
```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80

```
```
$ kubectl apply -f nginx-deployment.yaml

```
## 3. Which of these methods do you think is preferred and why?
* **editing yaml is better one since it syncs the yaml file in disk**
