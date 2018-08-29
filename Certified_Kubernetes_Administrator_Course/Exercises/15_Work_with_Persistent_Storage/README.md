# Work with Persistent Storage

## 1. On one of your cloud servers, install and configure Ubuntu 16 to be an NFS server.  Export a directory for use by the cluster.  Be sure you give all of your cluster nodes access to the directory.
```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install nfs-kernel-server -y
$ sudo mkdir /var/nfs/general -p

# set correct owner and group on the directory
$ sudo chown nodoby:nogroup /var/nfs/general

# edit file /etc/exports
$ sudo nano /etc/exports
# add "/var/nfs/general x.x.x.x(node1 interal ip address)(rw,sync,no_subtree_check) x.x.x.x(node2 interal ip address)(rw,sync,no_subtree_check) x.x.x.x(node3 interal ip address)(rw,sync,no_subtree_check) "

# restart nfs server
$ sudo systemctl restart nfs-kernel-server
```
## 2. Configure each of your Kubernetes nodes -- including the master -- as NFS clients.
```
# login to each nodes including master node
$ ssh user@x.x.x.x(node internal ip address)
$ sudo apt install nfs-common
```

## 3. Write the yaml to provision the storage in Kubernetes and call the API with it, and verify that the appropriate object has been created.
```
apiVersion: v1
kind: PersistentVolume
metadata:
 name: lab-vol
spec:
 capacity:
  storage: 1Gi
 volumeMode: Filesystem
 accessModes:
  - ReadWriteMany
 persistentVolumeReclaimPolicy: Recycle
 nfs:
  path: /var/nfs/general
  server: x.x.x.x(node ip address)
  readOnly: false
```
```
$ kubectl create -f pv.yaml
```
## 4. Write the yaml to request usage of the storage and call the API. Verify that the appropriate objects were created and/or allocated.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc  
spec:
 accessModes:
 - ReadWriteMany
 resources:
  requests:
   storage: 1Gi

```
```
$ kubectl create -f pvc.yaml
$ kubectl get pvc
$ kubectl get pv
# the status of pvc and pv are "Bound"

```
## 5. Write the yaml required by a busybox container to mount the volume at /tmp. Call the api.
```
apiVersion: v1
kind: Pod
metadata:
 name: nfs-pod
 labels:
  name: nfs-pod
spec:
 containers:
 - name: nfs-ctn
   image: busybox
   command:
    - sleep
    - "3600"
   volumeMounts:
   - name: nfsvol
     mountPath: /tmp
 restartPolicy: Always
 securityContext:
  fsGroup: 65534
  runAsUser: 65534
 volumes:
  - name: nfsvol
    persistentVolumeClaim:
     claimName: nfs-pvc
```
```
$ kubectl create -f nfs-pod.yaml
```
## 6. Invoke the shell on the busybox to interactively work with the container. Change to the /tmp directory and attempt to create a file called "its-alive.txt".
```
$ kubectl exec -it nfs-pod -- sh

$ cd /tmp
$ echo "hello!" -> its-alive.txt
$ exit
```
## 7. Verify that the file exists on the NFS server in the exported directory.
```
$ ls -la /var/nfs/general 
```
