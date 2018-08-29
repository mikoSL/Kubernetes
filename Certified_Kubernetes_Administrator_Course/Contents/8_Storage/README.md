# Storage

## 1. Persistent Volumes
* Native pod storage is ephemeral -- like a pod
* When a container crashes:
 1. Kubelet restarts it (possibly on another node)
 2. File system is re-created from image
 3. ephemeral files are gone!
* Docker Volumes:
 1. Directory on disk
 2. Possibly in another container
 3. New volume drivers
* Kubernetes volumes:
 1. Same lifetime as its pod
 2. Data preserved across container restarts.
 3. pod goes away --> volume goes away.
 4. Directory with data.
 5. Accessible to containers in a pod
 6. Implementation details determined by volume types.
* Using volumes
 1. Pod spec indicates which volumes to provide for the pod(spec.volumes)
 2. Pod spec indicates where to mount these volumes in containers (spec.containers.volumeMoutes)
 3. Seen from the containers's perspective as the file system.
 4. Volumes can not **mount onto other volumes**.
 5. **No hard links to other volumes**.
 6. Each pod must specify where each volume is mounted.
* Kubernets supported volume types, e.g.:
 1. awsElasticBlockStore(AWS EBS)
 ```
 $ aws ec2 create-volume --availability-zone=eu-west-la --size=10 --volume-type=gp2
 ```
 2. azureDisk and azureFile
 3. CephFS
 4. CSI(Container Storage Interface)
 5. local -- require "PersistentLocalVolumes" and "VolumeScheduling" feature
 6. persistentVolumesClaim(PVC)
 7. scaleIO
 8. secret volumes
 9. ...

## 2. Volumes and their access modes(RWO-ReadWriteOnce, ROX-ReadOnlyMany, RWX-ReadWriteMany)
* PersistentVolumes(PV) --**API** for users that **abstracts implementation details of storage**.
 1. Provisioned storage in the cluster.
 2. Cluster resource
 3. Volume plugins have **independent lifecycle from pods**.
 4. Volumes share the lifecycle of the pod. PC do **not**!
 5. API object(YAML) details the implementation.

* PersistentVolumeClaim(PVC) -- **method** for user to **claim durable storage** regardless of implementation details.
 1. Request for storage
 2. Pods consume node resource, **PVCs consume PV resources**.
 3. **Pods** can request **specific CPU and memory**. **PVS** can request **specific size and access modes**.
 4. PVCs also act as a "claim check" on a resource.  
 5. PVC protection ensure PVC actively in use not get removed from the system.
 ```
 $ kubectl describe pvc hostpath
 # check status: Terminating & Finalizers:[kubernetes.io/pvc-protection]

  ```
* PC vs PVCs
 1. users and application do not share identical requirement.
 2. Administrators should offer a variety of PVs without users worrying about the implementation details.
 3. PVs are cluster resource, PVCs are requests for the cluster resource.
 4. PVCs also act as a "claim check" on a resource.
 5. PVs and PVCs have a set lifecycle: provision --> bind --> reclaim

* Provision PV
 1. Static
 2. Dynamic

* Binding
 1. User create PVC
 2. Master watches for new PVCs and matches them to PVs.
 3. Master binds PVC and PV
 4. Volume may be more than the request.
 5. Binds are exclusive.
 6. PVC -> PV mapping is always 1:1.
 7. Claims not matches will remain unbound indefinitely.

* Reclaiming
 1. User can delete PVC object.
 2. Policies: Retain, Recycle, Delete.
 3. Administrator can configure custom recycler pod template.
 4. Administrator should configure **StorageClass** according to expectations.

```
$ kubectl expose deployment deployment-name --type="NodePort" --port 80
$ kubectl get services
$ curl localhost:32516
```
## 3. Application and Persistent storage (implement nfs storage solution to application having PV it needs)

1. Setup
```
# install nfs
$ sudo apt update
$ sudo apt install nfs-kernel-server

# make a directory to share with pods
$ sudo mkdir /var/nfs/general -p
$ ls -la /var/nfs/general

# DO NOT USE THIS IN PRODUCTION SERVER!!
$ sudo chown nobody:nogroup /var/nfs/general
$ ls -la /var/nfs/general

# export it
$ sudo nano /etc/exports
# write the following in the exports
/var/nfs/general xxx.x.x.x(ip_address of the node)(rw, sync, no_subtree_check) xxx.x.x.x(ip_address of the node)(rw, sync, no_subtree_check) xxx.x.x.x(ip_address of the node)(rw, sync, no_subtree_check)  xxx.x.x.x(ip_address of the node)(rw, sync, no_subtree_check)  
# save and quit

# sudo systemctl restart nfs-kernel-server

# prepare client , back to master node
$ sudo apt install nfs-common
$ ssh user@xx.x.x..(another worker node ip address)
$ sudo apt install nfs-common
# login to other workder node and install nfs-common
$ exit
```
2. provision storage, use the yaml
```
apiVersion: v1
kind: persistentVolumes
metadata:
 name: lapv
spec:
 capacity:
  storage: 1Gi
 volumeMode: Filesystem
 accessMode:
  - ReadWriteMany
 persistentVolumeReclaimPolicy: Recycle
 nfs:
  path: /var/nfs/general
  server: xx.x.x..x(node private ip address)
  readOnly: false
```

```
$ kubectl create -f pv.yaml
$ kubectl get pv
```
3. now it is the user who want to have PVC
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: nfs-PersistentVolumeClaim
spec:
 accessMode:
  - ReadWriteMany
 resources:
  requests:
   storage: 1Gi
```
```
$ kubectl create -f pvc.yaml
$ kubectl get pvc
```

4. connect pods
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
   volumeMoutes:
   - name: nfsvol
     mountPath: /tmp
 restartPolicy: Always
 securityContext:
  fsGroup: 65534
  runAsUser: 65534
 volumes:
  - name: nfsvol
    persistentVolumesClaim_
     claimName: nfs-pvc
```

```
$ kubectl create -f nfs-pod.yaml
$ kubectl get pods -o wide

# check details
$ kubectl exec -it nfs-pod -- sh
/ $ cd /tmp
/tmp $ ls -la
# add a file and check if the file is stored 
/tmp $ touch hello-from-a-pod

$cd /var/nfs/general
$ ls -la
```
