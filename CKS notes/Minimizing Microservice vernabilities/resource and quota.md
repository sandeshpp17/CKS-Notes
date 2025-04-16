Resource and quota is the method the dividing resource for specific teams. enforce the resource-quota we can provide the limited amount of resource to different team. minimizing cost of infrastructure. 

To create the resource-quota for team with their namespace.

```shell
cat <<EOF > compute-resources.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: team-a
spec:
  hard:
	pods: "5"
    requests.cpu: "1"
    requests.memory: "1Gi"
    limits.cpu: "2"
    limits.memory: "2Gi"
    requests.nvidia.com/gpu: 4
EOF
```

create this and no pods will exceed this limit i will show error before creating pod if pod exceed the resource and quota limit.

example of pod within defined resource and quota
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: high-priority
spec:
  containers:
  - name: high-priority
    image: ubuntu
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10;done"]
    resources:
      requests:
        memory: "500Mi"
        cpu: "0.1"
      limits:
        memory: "1Gi"
        cpu: "0.5"
  priorityClassName: high
```