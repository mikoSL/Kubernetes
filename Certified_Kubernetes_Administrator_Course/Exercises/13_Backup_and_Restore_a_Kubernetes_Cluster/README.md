# Backup and restore a Kubernetes cluster / Maintain on Node 3

## 1. Prepare node 3 for maintenance by preventing the scheduler from putting new pods on to it and evicting any existing pods. Ignore the DaemonSets -- those pods are only providing services to other local pods and will come back up when the node comes back up.
```
$ kubectl drain node3-name --ignore-daemonSets
```

## 2. When you think you've done everything correctly, go to the Cloud Servers page and shut node 3 down. Don't delete it! Just stop it. While it's down, we'll pretend that it's getting that new multiverse battery. While you wait for the cluster to stabilize, practice your yaml writing skills by creating yaml for a new deployment that will run six replicas of an image called k8s.gcr.io/pause:2.0. Name this deployment "lots-of-nothing".


```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
 name: lots-of-nothing
spec:
 selector:
  matchLabels:
   timeToGet: schwifty
 replicas: 6
 template:
  metadata:
   labels:
    timeToGet: schwifty
  spec:
   containers:
   - name: pickle-rick
     image: k8s.gcr.io/pause:2.0

```

## 3. Bring the "lots-of-nothing" deployment up on your currently 75% active cluster. Notice where the pods land.
```
$ kubectl create -f lots-of-nothing.yaml
$ kubectl get pods -o wide -l timeToGet=schwifty
```

## 4. Imagine you just got a text message from the server maintenance crew saying that the installation is complete. Go back to the Cloud Server tab and start Node 3 up again. Fiddle with your phone and send someone a text message if it helps with the realism.  Once Node 3 is back up and running and showing a "Ready" status, allow the scheduler to use it again.
```
$ kubectl get nodes
$ kubectl uncordon nodes3-name
```

## 5. Did any of your pods move to take advantage of the additional power?  
* NO! the uncordon only affects pods being scheduled and will not move any back unless other nodes are experiencing MemoryPressure or DiskPressure.
