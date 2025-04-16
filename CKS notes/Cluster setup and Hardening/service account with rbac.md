
create the service account for 3rd party app like Jenkins.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
  namespace: default
secrets:
  - name: build-robot-secret # usually NOT present for a manually generated token
```

note that the secret token is not generated you have to create it for service account. 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: build-robot-secret
  namespace: default
  labels:
    kubernetes.io/legacy-token-last-used: 2022-10-24
    kubernetes.io/legacy-token-invalid-since: 2023-10-25
  annotations:
    kubernetes.io/service-account.name: build-robot
type: kubernetes.io/service-account-token
```



once both create check the token and decode it

now creating RBAC like role and role-binding.

creating role
```
kubectl -n default create role rolename --verb=<get,list,watch> --resource=<pods,deploy> 
```

creating role-binding
```
kubectl -n default create rolebinding rolebindingname --role=rolename --serviceaccount=namespace:serviceaccountname
```



