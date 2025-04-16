kubelet works like captain in the k8s cluster handling the pod creating,deletion and update.
securing kubelet is best practice.
kubelet conf passed --config use ps -aux | grep kubelet and look for --conf for path of kubelet configuration file.

it depends how you configure your k8s cluster. if using hard way you have to set the ssl encryption with kubeapi and kubelet for secure communication. 

| port  | Description                                            |
| ----- | ------------------------------------------------------ |
| 10250 | serves api that allow full access                      |
| 10255 | serves api that allow unauthenticated read only access |
### Authentication to kubelet
to get the pods and its full access curl https://localhost:10250/pods it will return the list of pod.
if you configure the kubelet with kubeadm tool it will handle the ssl encryption by default. 
but kubelet allows insecure communication 'curl -sk https://localhost:10250/pods'

To disable the kubelet insecure behavior use flag anonymous-auth to false. 

```
apiVersion: kubelet.config
kind: KubeletConfiguration
authentication:
	anonymous:
		enabled: false
```

### Authorization to kubelet

Any request that is successfully authenticated (including an anonymous request) is then authorized. The default authorization mode is `AlwaysAllow`, which allows all requests.

to sub divide the access to kubelet we use the authorization to kubelet by setting the authorization mode from AlwaysAllow to Webhook
start the kubelet with the `--authorization-mode=Webhook` and the `--kubeconfig` flags

### read only port 10255 
by default the read only port is disable on most of cluster. if you find this port is enabled `--read-only-port=10255` and you can disable it by using the flag `readOnlyPort: 0`


https://kubernetes.io/docs/reference/access-authn-authz/kubelet-authn-authz/