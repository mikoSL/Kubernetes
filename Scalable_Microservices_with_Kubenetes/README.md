# Scalable Microservices with Kubernetes from udacity.com

## 1. Secret and Configmaps

1. Create secrets
```
$ ls tls/

# create a secret
$ kubectl create secret generic tls-certs --from-file=tls/

$ kubectl describe secrets tls-certs

# review the config file
$ cat nginx/proxy.conf

# create Configmaps
$ kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf

$ kubectl describe configmap nginx-proxy-conf
```
2. Access a Secure HTTPs end

```
$ Examine the secure-monolith pod config file (nginx and monolith are in the same pods because they are dependent)
$ cat pods/secure-monolith.yaml

# create a secret
$ kubectl create secret generic tls-certs --from-file=tls/

$ kubectl describe secrets tls-certs

# review the config file
$ cat nginx/proxy.conf

# create secure monolith pod
$ kubectl create -f pods/secure-monolith.yaml
$ kubectl get pods secure-monolith
```
```
# open another terminal
$ kubectl port-forward secure-monolith 10443:443

$ curl --catert tls/ca.pem https://127.0.0.1://10443

# check logs
$ kubectl logs -c nginx secure-monolith
```

## 2. Service (persistent endpoint for pods using label)
* cluster IP(internal only)
* Type NodePort (external IP to access)
* node load balancer

```
$ cat services/monolith.yaml
```

```
apiVersion: v1
kind: Service
metadata:
 name: "monolith"
spec:
 selector:
  app: "monolith"
  secure: "enabled"
 ports:
  - protocol: "TCP"
    port: 443
    targetPort: 443
    nodPort: 31000
 type: NodePort
```

```
$ kubectl create -f service/monolith.yaml

$ gcloud compute fireware-rules create allow-monolith-nodeport --allow=tcp:31000

$ kubectl label pods secure-monolith "secure=enabled"
$ kubectl describe pods secure-monolith | grep Labels

$ kubectl describe services monolith | grep Endpoints

# get the exteranal IP, copy one for test with curl
$ gcloud compute instances list
$ curl -k https://x.x.x.x:31000

```

## 3. Deployment(drive current state towards desired state)
```
$ kubectl create -f deploytment/auth.yaml
$ kubectl create -f service/auth.yaml

$ kubectl create -f deploytment/hello.yaml
$ kubectl create -f service/hello.yaml

$ kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
$ kubectl create -f deployment/frontend.yaml
$ kubectl create -f service/frontend.yaml

# interact with frontend by grabbing external IP address
$ kubectl get services frontend

# copy the IP address from above
$ curl -k https://x.x.x.x
```

## 4. Scaling & RollingUpdate
```
$ kubectl get replicasets

$ kubectl get pods -l "app=hello, track=stable"

$ vi deployment/hello.yaml
# change the replica in the yaml from 1 to 3

$ kubectl apply -f deployment/hello.yaml
$ kubectl describe deployment hello
```
