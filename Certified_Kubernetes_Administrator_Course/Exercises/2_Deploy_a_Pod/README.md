# This pod will cause the alpine linux container to sleep 3600 seconds then exit. Kubernetes will restart the pod.

## 0. Open master node
1. create yaml file
```
$ vi alpine.yaml

```
2. write yaml file
```
apiVersion: v1
kind: Pod
metadata:
  name: alpine
  namespace: default
spec:
  containers:
  - name: alpine
    image: alpine
    command:
      - sleep
      - "3600"
    imagePullPolicy: IfNotPresent
  restartPolicy: Always

```
3. create this yaml file in master node
```
$ kubectl create -f alpine.yaml

```
4. check the status of the job  
```
$ kubectl describe job pi

```
5. when job is finished, view the result in the pod  
```
$ kubectl logs pi-xxxx

```
