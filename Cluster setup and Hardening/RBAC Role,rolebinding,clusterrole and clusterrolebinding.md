Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization.

RBAC authorization uses the `rbac.authorization.k8s.io` [API group](https://kubernetes.io/docs/concepts/overview/kubernetes-api/#api-groups-and-versioning) to drive authorization decisions, allowing you to dynamically configure policies through the Kubernetes API.

## Role and Cluster-role

role and cluster-role are the set of permission for the resources. 
role is the namespace resource and it can only provide the access of resources within the namespace.
cluster-role is non-namespace resource and it can provide the access of resource across the namespace. 

to create role use below command
```shell
kubectl create role pod-reader --verb=get --verb=list --verb=watch --resource=pods
```

To create cluster role use below command

```shell
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=po
```

## Role-binding and Cluster-role-binding

As name states the role-binding binds the with role providing access to user,groups and service accounts. A RoleBinding grants permissions within a specific namespace whereas a ClusterRoleBinding grants that access cluster-wide.

Example of role-binding
```shell
kubectl create rolebinding bob-admin-binding --clusterrole=admin --user=bob --namespace=acme
```

Example of cluster role binding
```shell
kubectl create clusterrolebinding root-cluster-admin-binding --clusterrole=cluster-admin --user=root
```

Example with service account
```shell
kubectl create rolebinding my-sa-view \
  --clusterrole=view \
  --serviceaccount=my-namespace:my-sa \
  --namespace=my-namespace
```