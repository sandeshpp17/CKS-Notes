container uses the host os kernel and make system call with hardware.
if any container is venerable in anyways then it would make whole system down. To avoid this situation container sandboxing is used.

gVisor and kata container are the example of container sandboxing, 
both create a kernel layer at container level and its serve the kernel capability.

## Container runtime.
To run or create the container, container runtime is need dockerd,containerd and podman are the example of container runtime.
runc is the underlying component which creates container. docker uses it to create container. runc is the initiative of oci open container initiative.

### using gvisor as container runtime for sandboxing.

To list the runtime installed in k8s

```
k get runtimeclasses
```

creating the runtime class.

```yaml
# RuntimeClass is defined in the node.k8s.io API group
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  # The name the RuntimeClass will be referenced by.
  # RuntimeClass is a non-namespaced resource.
  name: myclass 
# The name of the corresponding CRI configuration
handler: myconfiguration 
```

using runtime class in pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  runtimeClassName: myclass
  # ...
```


==Note==: only some of the hypervisor only support the container sandboxing for gVisor and kata continer.
