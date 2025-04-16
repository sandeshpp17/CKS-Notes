
Consider you have multiple pod of same application which critical and other application which has same number of less critical pods. by defining the priority we can ensure that critical pod gets deployed 1st in cluster at high priority than less critical pod.

# API priority and Fairness.

To enable the priority and fairness you have to add the below flag in kube-api server config yaml

```shell
kube-apiserver \
--runtime-config=flowcontrol.apiserver.k8s.io/v1beta3=false \
 # …and other flags as usual
```

The command-line flag `--enable-priority-and-fairness=false` will disable the API Priority and Fairness feature.

Below is the flow-schema which will be applied for enabling the priority with API.

```yaml
apiVersion: flowcontrol.apiserver.k8s.io/v1
kind: FlowSchema
metadata:
  name: list-events-default-service-account
spec:
  distinguisherMethod:
    type: ByUser
  matchingPrecedence: 8000
  priorityLevelConfiguration:
    name: catch-all
  rules:
    - resourceRules:
      - apiGroups:
          - '*'
        namespaces:
          - default
        resources:
          - events
        verbs:
          - list
      subjects:
        - kind: ServiceAccount
          serviceAccount:
            name: default
            namespace: default
```

## Pod priority and preemption

To set priority for pods 1st we need to create priority class and then set that priority class in pod.

```
 # Create a priority class named default-priority that is considered as the global default priority
  kubectl create priorityclass default-priority --value=1000 --global-default=true --description="default priority"
```

To set the priority for pod 
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
  priorityClassName: default-priority
```

API: https://kubernetes.io/docs/concepts/cluster-administration/flow-control/

POD: https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/