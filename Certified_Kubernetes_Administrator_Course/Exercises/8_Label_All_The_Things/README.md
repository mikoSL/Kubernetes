# Label all the things

## 1. Label each of your nodes with a "color" tag. The master should be black; node 2 should be red; node 3 should be green and node 4 should be blue

```
$ kubectl label node master-node-name color=black
$ kubectl label node node2-name color=red
$ kubectl label node node3-name color=green
$ kubectl label node node4-name color=blue
```
## 2. If you have pods already running in your cluster in the default namespace, label them with the key/value pair running=beforeLabels. Flag **-n** (**namespace**). Flag **-all**.
```
$ kubectl label pods -n default running=beforeLabels --all
```

## 3. Create a new alpine deployment that sleeps for one minute and is then restarted from a yaml file that you write that labels these container with the key/value pair running=afterLabels.
```
apiVersion: v1
kind: Pod
metadata:
 name: alpine
 namespace: default
 labels:
  running: afterLabels
spec:
 containers:
 - name: alpine
   image: alpine
   command:
    - sleep
    - "60"
 restartPolicy: Always
```

## 4. List all running pods in the default namespace that have the key/value pair running=beforeLabels.
```
$ kubectl get pods -l running=beforeLabels -n default
```

## 5. Label all pods in the default namespace with the key/value pair tier=linuxAcademyCloud.
```
$ kubectl label pods --all -n default tier=linuxAcademyCloud
```

## 6. List all pods in the default namespace with the key/value pair running=afterLabels and tier=linuxAcademyCloud.
```
$ kubectl get pods -l running=afterLabels,tier=linuxAcademyCloud
```
