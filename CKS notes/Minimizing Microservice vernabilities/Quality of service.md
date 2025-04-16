Kubernetes uses QoS classes to make decisions about evicting Pods when Node resources are exceeded.

When Kubernetes creates a Pod it assigns one of these QoS classes to the Pod:

- [Guaranteed](https://kubernetes.io/docs/concepts/workloads/pods/pod-qos/#guaranteed)
- [Burstable](https://kubernetes.io/docs/concepts/workloads/pods/pod-qos/#burstable)
- [BestEffort](https://kubernetes.io/docs/concepts/workloads/pods/pod-qos/#besteffort)


https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/