# Quiz

1. In typical deployment, kubernetes master listens on port **443**(the secure HTTP port).

2. Docker volume lifetime is loosly defined. Kubernetes volume has the same lifetime as its surrounding pod.

3. **MemoryPressure** and **DiskPressure** are return true if memory is low when running node.

4. **VxLANs** is the underlying technology Flannel uses to allow pods to communicate.

5. An appropriate **CNI(Container Network Interface)** should be chosen to deploy kubernetes using kubeadm.

6. **Ceph** is an object store, not a CNI provider.

7. For network policies to work in Kubernetes, **CNI must enforce** the network policies.If the CNI doesn't support network policies, then applying a YAML formula with a network policy in it will return a success, but the policies will not be enforced.

8. Kubernetes pods 'restart policies': **Always**, **OnFailure** and **Never**.

9. **DaemonSet** (shotform **ds**)use case: An **CNI container** that needs to run on **every node** in order to function properly. Use DeamsonSet for Pods that need to run **one per machine**, because they provide a **machine-specific system** service.

10. All the ways of **assigning a pod to a particular node** involve **lables**, they include **kubectl** or **Kubenetes API calls**.  

11. Limit pod CPU utilization to one quater of CPU, using **cpu:250m**, 250m stands for 250 millicups which works out to 1/4 of a running CPU.

12. **Labels** are used to select and identify objects. **Annotations** allow for wider variety of characters that labels do not allow. Both labels and annotations use key/value pair config maps.

13. Annotation is important especially when using **multiple or custom schedulers** because they can **remind operators which scheduler was used to place (or fail to place) a pod**.

14. **Scheduler** is a pod on the master node. It is a process that runs in a pod, unusually on a the master node. While it is unusual, it is possible to have multiple scheduler running on the same cluster.

15. When an API request is make to create a pod, **scheduler** is used to determine which node will be used to instantiate the new pod.

16. **nodeSelector** is used to assign a pod to a specific node. Label that node first.

17. Configure an application in a container from Kubernetes by using **environment variables**.

18. Kubernetes API object hierarchy: **Pods** make up **deployment**. **Service** point to **deployment**.

19. The reason why a user desire two pods to have **anti-affinity** is she want them to run on different nodes to avoid sharing failure domains. **Anti-affinity** means that two pods will not run on the same node, and is usually implemented to prevent two pods from being in the same failure domain in case sth goes wrong.

20. **Taints** are used to **repel certain pods** from nodes and are applied to nodes. Taints allow a node to repel a set of pods.  

21. If a pod requests more resource than **is available** on **any** given node, the pod will **not be scheduled** until a node with the resources becomes available.

22. **Through the schedulerName tage(default is default-scheduler)** in spec, a user can specify which scheduler a pod should use.

23. **PodAffinity** is used for placing two ore more pods on the same node. (label is used as well with podAffinity attribute)

24. If **a toleration and a taint** match during scheduling, the taint is ignored and the pod might be scheduled to the node. Taint and toleration work together to make sure pods are not scheduled onto inappropriate nodes. One ore more taints are applied to a node, this marks that the node should not accept any pods that do not tolerate the taints. Tolerations are applied to pods, and allow(but do not require) the pods to schedule onto nodes with matching taints.

25. Shell prompt to a Ubuntu 16.04 based container called "sidecar1" in the pod "star-aaa-bb"? There are serveral containers in the pod. **$ kubectl exec -it star-aaa-bb --container sidecar1 --/bin/bash** It is not recommended to get to a containers's shell in Kubenetes, use it for debugging only.

26. Check the application logs, which are in a file in the container "appctn" in the pod "apppod-abc-123" at /var/log/applog. **$ kubectl exec apppod-abc-123 --cat /var/log/applog**

27. **$ kubectl logs to-the-screen** give you **stdout(standard output) of a pod** called "to-the-screen" .

28. Kubernetes key/value store(etcd) log file reside in **/var/log/pods**

29. Check the CPU and memory request of a node called "node8" **$ kubectl describe node node8**

30. **flag --previous** will ge the logs back from a **dead or evicted* pod**.

31. Easy command to check the health and status of your cluster **$ kubectl get nodes**

32. **$ kubectl top node-name pod-name** command line can access to the mew metrics API which started with Kubernetes1.8.

33. Drain one node at a time, if you **attempt to drain more**, any drains that would **cause the number of ready replicas to fall below the specified budget** are **blocked**. The **stateful Sets** take care of themselves and prevent drains from happening that would prevent them from maintaining their budgets.

34. Upgrading is interrupted some nodes are in older version, some in new. The upgrading can be re do by running kubeadm. It is **idempotent** and will more all nodes to be desired state.

35. Using latest version of Kubernetes but disallow any features that are not part of v1 API. append **--runtime-config=api/all=false, api/v1=true** to the command that bring up his apiserver. **apiserver is the only piece that requires configuration to this possible**.

36. **Node self registration mode** can easily add nodes to your cluster. When the kubelet flag **--register-node** is true(the default), kubelet will attempt to register itself with AIP server. This the prefered pattern, used by most distros.  

37. **Ingress** allows us to abstract away the implementation details of routes into the cluster, such as load balancers. An API object aht manages external access to the services in a cluster, usually HTTP.

38. If an ingress request is made with **no associated rules**, all traffic is sent to a **single host**. This is a useful way of setting up common error pages, such as the location of a unified 404 pages.

39. **CNI** handles inter-pod communication. It allows **pods** to communicate with one another **within a cluster** regardless of which node they are on.

40. YAML for a **network policy**, the pattern is **Preamble, podSelector, ingress and/or engress rules**. Preamble contains apiVersion, kind, metadata. podSelector to determine which pods this policy oversees and the rules.

41. If a service called "web-head" is exposed in the default namespace, then other pods can resolve it using all of these hostnames except **web-head.local**. The .local will not work!

42. **Network policies** determines how a set of pods are allowed communicate with one another and other network endpoints. It determine what traffic gets into and out of a pod. CNI must support them, though, but not of them do.

43.  For a user to be able to request an ingress resource, cluster must have an ingress controller compatible with available and appropriate service providers like load balancers. With kubernetes, the general rule of thumb is that YAML requests will return successfully, but if there is no service to fulfill it then the request will have **no effect**.
