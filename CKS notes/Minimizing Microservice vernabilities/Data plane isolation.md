Dataplane isolation for network can by performed by [[Network Policy]]. 

Dataplane isolation for storage.
to divide the storage we can create storage-class and using different storage class it can be assigned to different user.

## Dataplane isolation with network policy.

in below example if we want to allow traffic form only specific namespace.
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-specific-ingress
  namespace: namespace-ui
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          namespace: namespace-web
```

deny external egress traffic but enable internal egress for the pod.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-external-egress
  namespace: namespace-worker
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector: {}
      podSelector: {}
```


## Data plane isolation on node using taints and toleration.

to isolate the data plan using taint and toleration, we can implement the necessary pods only run of selected node. if some pod are on high resource requirement then we can apply the taint on node and toleration on pod so that only tolerated pod can run on tainted node.

To taint the node 
```shell
kubectl taint nodes node1 key1=value1:NoSchedule
```

for toleration of pod 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "key1"
    operator: "Equal"
	value: "value1"
	effect: "NoSchedule"
```