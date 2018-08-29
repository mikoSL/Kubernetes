# Security

## 1. Authentication & Authorization
* TLS(Transport Layer Security) established
* Authentication (Authenticator modules)
* Admissions modules
* Authorization (ABAC, RBAC, Webhook)
* **Kubelet authentication and authorization**: Kubelet's endpoint exposes APIs which give access to dat of varying sensitivity.

## 2. Configure network policies
* Specification of how groups of pods may communicate.
* use labels to select pods and define rules.
* Pods are non-isolated by default.
* Pods are isolated when a network policy selects them: **podSelector** and **policyTypes: Ingress and Egress**

## 3. TLS certifications for cluster components

```
$ cd k8s/easy-rsa-master/easyrsa3/
$ ./easyrsa init-pki

# generate certificate authority
$ ./easyrsa --batch "--req-cn=${MASTER_IP}@'date + %s'" build-ca nopass

$ cat ~/k8s/rsa-request.sh
#copy content from above and paste --> create

# cd pki
$ ls -la

$ cd issued
$ ls -la

$ cd ../private/
$ ls -la

$ ps aux | grep aipserver

```

## 4. Securing images (vulnerabilities scanning)
* regular apply security updates to environment.
* **DO NOT** run tool like **apt-update** in containers.
* Use **rolling update**
* Authorized image
* Private registries to store approved images.
* CI pipeline should ensure that only vetted cod is used for building images.

## 5. Define security contexts (privileged containers, Privilege Escalation)
```
apiVersion: v1
kind: Pod
metadata:
 name: security-context-pod
spec:
 securityContext:
  runAsUser: 1000
  fsGroup: 2000
 volumes:
 - name: sam-vol
   emptyDir: {}
 containers:
 - name: sample-container
   image: gcr.io/google-samples/node-hello:1.0
   volumeMounts:
   - name: sam-vol
     mountPath: /data/demo
   securityContext:
    allowPrivilegeEscalation: false
```
```
$ Kubectl create -f security.yaml

$ Kubectl exec -it security-context-pod -- sh
$ ps aux # user is 1000

$ cd /data
$ ls -la # demo user is 2000

$ cd demo
$ echo Studying > test
$ ls -la # test user is now 1000
```

* Which one has higher priority? runAsUser in **pod** or runAsUser in **container**. Add runAsUser in container. **runAsUser in container** has higher priority.
```
apiVersion: v1
kind: Pod
metadata:
 name: security-context-pod
spec:
 securityContext:
  runAsUser: 1000
  fsGroup: 2000
 volumes:
 - name: sam-vol
   emptyDir: {}
 containers:
 - name: sample-container
   image: gcr.io/google-samples/node-hello:1.0
   volumeMounts:
   - name: sam-vol
     mountPath: /data/demo
   securityContext:
    runAsUser: 2000
    allowPrivilegeEscalation: false
```
