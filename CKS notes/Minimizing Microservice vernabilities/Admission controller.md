When a user initiate the request with kubectl the request goes through authentication -> authorization -> admission controller -> resource-create. Once the authentication and authorization is completed if the request need other resource to be created then the request is rejected.
for example, if we try to create pod with test namespace and if the test namespace is not present then admission controller rejects that request.

Some of the admission controller are not enable by default we can enable it in kube-api yaml config file. using namespaceautoprovision the new namespace is created automatically required by resource.


To enable or disable admission plugins edit /etc/kubernetes/manifests/kube-api.yaml or edit the service.

```
--enable-admission-plugins=ValidatingAdmissionWebhook,MutatingAdmissionWebhook
```

```shell
kube-apiserver --disable-admission-plugins=PodNodeSelector,AlwaysDeny 
```


To check which plugins are enabled 
```shell
kube-apiserver -h | grep enable-admission-plugins
```