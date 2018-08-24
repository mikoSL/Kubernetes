# Setting container environment variables

## 1. Write yaml for a job that will run the busybox image and will print out its environment variables and shut down.

```
apiVersion: v1
kind: Pod
metadata:
 name: env-dump
spec:
 containers:
 - name: busybox
   image: busybox
   command:
    - env

```
## 2.Add the following environment variables to the pod definition:

* "STUDENT_NAME="Your Name"
* "SCHOOL="Linux Academy"
* "KUBERNETES="is awesome"

```
apiVersion: v1
kind: Pob
metadata:
 name: env-dump
spec:
 container:
 - name: busybox
   image: busybox
   command:
    - env
   env:
   - name: STUDENT_NAME
     value: "Your Name"
   - name: SCHOOL
     value: "Linux Academy"
   - name: KUBERNETES
     value: "is awesome"

```

## 3. Run the job

```
$ kubectl create -f env-dump.yaml

```

## 4. Verify that the environment variables were added.
```
$ kubectl logs env-dump

```
