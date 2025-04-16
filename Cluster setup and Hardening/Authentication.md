
k8s do not manage the authentication and authorization directly it take the help of other iam like LDAP, etc.

there are 2 method the take access of cluster using admin user and using service account for third party application like jenkins, 3rd party application.



## service account.

service account is generally used for access resources of kubernetes cluster. by making call with kube-api server. its uses secret token to gain access of cluster via service account.

as per latest update.
1. the token is not automatically generated.
2. you have to disable auto-mounting of ServiceAccountToken by using parameter `automountServiceAccountToken: false` 


steps:

1. create service account.
```
kubectl create sa <sa-name>
```

2. to create token.
```shell
kubectl create token build-robot
```
3. pod manifest 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /var/run/secrets/tokens
      name: vault-token
  serviceAccountName: build-robot # < --pass the name of you sa 
  volumes:
  - name: vault-token
    projected:
      sources:
      - serviceAccountToken:
          path: vault-token
          expirationSeconds: 7200
          audience: vault
```
