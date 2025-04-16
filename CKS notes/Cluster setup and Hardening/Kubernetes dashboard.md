a way to access the k8s resource via gui.

k8s dashboard is not installed by default in k8s. you have the install it in the kubernetes dashboard namespace and service is ClusterIP so you can access it using the kubernetes proxy. 
refer this link to install dashboard in cluster [k8s-dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

use below command to create k8s dashboard

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

below command is used to access kubernetes dashboard using kube proxy.
```
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

you can also access the dashboard via service setting as NodePort.

### Accessing the k8s dashboard using kubernetes token.

user can access the k8s dashboard using service account. when you enable to dashboard access using proxy there are 2 methods to login.
1. Token
2. kubeconfig

### Access using Token 
use the token created in the kubernetes dashboard with the permission of admin-user role. 
admin-user the role cluster admin role which has all cluster related permission. its bind with the role-binding with service account.

To get the token of service account check the secret attached to it.

```
k -n kubernetes-dashboard get sa
```

check the secret of service account
```
k -n kubernetes-dashboard describe sa dashboard-serviceaccount 
```

extract the secret token and decode it with base64 
```
k -n kubernetes-dashboard get secret dashboard-secret -o jsonpath='{.data.token}' | base64 --decode > secret-token.txt
```

use this token to access the k8s dashboard.


### Access using kubeconfig.

the path to kubeconfig file is `~/.kube/config` using this file to access the dashboard. it will have the admin permission.

### Securing the k8s dashboard with less permission service account.

- create a new service account.
- check the clusterrole with view permission or admin permission.
- create rolebinding with the clusterrole view and service account.
- now create clusterrolebinding and attach with role list-namespace and service account. 

Note: the effect of this service account will be user only able to access resource within the namespace for which he has access and able to list all namespaces. with view role he is able to get readonly access to cluster. 