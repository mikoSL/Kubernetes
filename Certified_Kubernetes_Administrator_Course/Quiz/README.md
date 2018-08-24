# Quiz

1. In typical deployment, kubernetes master listens on port **443**(the secure HTTP port).

2. Docker volume lifetime is loosly defined. Kubernetes volume has the same lifetime as its surrounding pod.

3. **MemoryPressure** and **DiskPressure** are return true if memory is low when running node.

4. **VxLANs** is the underlying technology Flannel uses to allow pods to communicate.

5. An appropriate **CNI(Container Network Interface)** should be chosen to deploy kubernetes using kubeadm.

6. **Ceph** is an object store, not a CNI provider.

7. For network policies to work in Kubernetes, **CNI must enforce** the network policies.If the CNI doesn't support network policies, then applying a YAML formula with a network policy in it will return a success, but the policies will not be enforced.

8. kubernetes pods 'restart policies': **Always**, **OnFailure** and **Never**.

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

24. If **a toleration and a taint** match during scheduling, the taint is ignored and the jpod might be scheduled to the node. Taint and toleration work together to make sure pods are not scheduled onto inappropriate nodes. One ore more taints are applied to a node, this marks that the node should not accept any jpods that do not tolerate the tains. tolerations are applied to pods, and allow(but do not require) the pods to schedule onto nodes with matching taints.
