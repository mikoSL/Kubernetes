# Lable a node & schedule a pod

## 1.Pretend that node 3 is your favorite node.  Maybe it's got all SSDs.  Maybe it's got a fast network or a GPU.  Or maybe it sent you a nice tweet.  Label this node in some way so that you can schedule a pod to it.

```
$ kubectl label node node3-name net=fast

```
## 2. Create a yaml for a busybox sleeper/restarter that will get scheduled to your favorite node.
```
apiVersion: v1
kind: Pod
metadata:
 name: busybox
 namespace: default
spec:
 containers:
 - name: busybox
   image: busybox
   command:
    - sleep
    - "300"
   imagePullPolicy: IfNotPresent
 restartPolicy: Always
 nodeSelector:
  net: fast
```
