Attribute-based access control (ABAC) defines an access control paradigm whereby access rights are granted to users through the use of policies which combine attributes together.

below is the syntax to set ABAC for k8s and the file should present in below path

/etc/kubernetes/abac/<yourfile.yaml>

```json
{"apiVersion":"abac.authorization.kubernetes.io/v1beta1","kind":"Policy","spec":{"user":"system:serviceaccount:kube-system:default","namespace":"*","resource":"*","apiGroup":"*","readonly": true}}
```

Pass below flags in `/etc/kubernetes/manifests/kube-apiserver.yaml` to active abac
 ```
- --authorization-mode=Node,RBAC,ABAC
- --authorization-policy-file=/etc/kubernetes/abac/abac-policy.jsonl
```
