# Multiple scheduler

## Pods generally are scheduled by the default scheduler, and their yaml might look like this:

```
apiVersion: v1
kind: Pod
metadata:
  name: annotation-default-scheduler
  labels:
    name: multischeduler
spec:
  schedulerName: default-scheduler
  containers:
  - name: pod-container
    image: k8s.gcr.io/pause:2.0

```
* Ordinarily, we don't need to specify the scheduler's name in the spec because everyone uses a single default one. Sometimes, however, developers need to have custom schedulers in charge of placing pods due to legacy or specialized hardware constraints.

## 2. Rewrite the yaml for this pod so it makes use of a scheduler called custom-scheduler, and annotate the pod accordingly.
```
apiVersion: v1
kind: Pod
metadata:
  name: annotation-default-scheduler
  labels:
    name: multischeduler
  annotations:
    scheduledBy: custom-scheduler
spec:
  schedulerName: custom-scheduler
  containers:
  - name: pod-container
    image: k8s.gcr.io/pause:2.0
```
