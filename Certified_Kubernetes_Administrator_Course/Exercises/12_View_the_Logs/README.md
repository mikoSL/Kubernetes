# View the logs

## A small container which wakes up every three seconds and logs the date and a count to stdout
```
apiVersion: v1
kind: Pod
metadata:
  name: counter
  labels:
    demo: logger
spec:
  containers:
  - name: count
    image: busybox
    args: [/bin/sh, -c, 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 3; done']

```

## 2. View the current logs of the counter.
```
$ vi counter.yaml
# copy the above yaml, save file and exist.

$ kubectl create -f counter.yaml
$ kubectl logs counter
```

## 3. Allow the container to run for a few minutes while viewing the log interactively.
```
$ kubectl logs counter -f
```

## 4. Have the command only print the last 10 lines of the log.
```
$ kubectl logs counter --tail=10
```

## 5.Look at the logs for the scheduler. Have there been any problems lately that you can see?
```
$ cd /var/log/contains
$ ls
# the one starts with "kube-scheduler" are the logs
$ sudo cat kube-scheduler-xxxx.com_kube-system_kube-scheduler-xxxxx.log
```

## 6.Kubernetes uses etcd for its key-value store. Take a look at its logs and see if it has had any problems lately.
```
$ cd /var/log/contains
$ ls
# the one starts with "etcd" are the logs
$ sudo cat etcd-xxx.com_kube-system_etcd-xxxxx.log
```

## 7.Kubernetes API server also runs as a pod in the cluster. Find and examine its logs.
```
$ cd /var/log/contains
$ ls
# the one starts with "kube-apiserver" are the logs
$ sudo cat kube-apiserver-xxx.com_kube-system_kube-apiserver-xxxxx.log 
```
