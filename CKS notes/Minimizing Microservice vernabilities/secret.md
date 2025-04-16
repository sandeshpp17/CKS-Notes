Secrets are used to pass the sensitive data in pod or container as env variable.

## Two type of passing secret
1. imperative 
	use command to create secret.
```shell
kubectl create secret generic test-secret --from-literal='username=my-app' --from-literal='password=39528$vdg7Jb'
```
1. declarative
	use yaml file to create secret.
	1st encode the secret with base64 and then create secret
```shell
echo -n 'my-app' | base64
echo -n '39528$vdg7Jb' | base64
```
below is yaml with encoded key value
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: test-secret
data:
  username: bXktYXBw
  password: Mzk1MjgkdmRnN0pi
```

use below pod example to pass secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envfrom-secret
spec:
  containers:
  - name: envars-test-container
    image: nginx
    envFrom:
    - secretRef:
        name: test-secret
```


reference: https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/


# Encrypting Secret

secret is encoded no encrypted anyone can decode the secret with base64.

To encrypt the secret follow below etcd resource encryption.

https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/