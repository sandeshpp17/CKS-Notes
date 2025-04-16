Securing the node metadata is crucial it store the information of node in the form the key value pair.

if any attacker finds out that you are running the old version of kubelet and kernel or get the IP address of node. then attacker can use version specific attack to k8s cluster.

## secure with network policy
To secure the node metadata we can implement the [[Network Policy]].
use the egress deny policy 

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress
spec:
  podSelector: 
	  matchLabels:
		  key: value
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - <nodeip>/32 # block from specific node
```

## Secure with service account.

when the pod is exposed to public it will use the service account. if the pod service account have clusterrole then its can list the node metadata. 
so, you have identify which pod uses the clusterrole and the clusterrole resource is node then its determined that service account is having more permission.

after identification you can delete the service account.

```
k -n default delete pod test-api-pod
k -n default delete sa node-sa
k delete clusterrole node-role
k delete clusterrolebinding node-role-binding
```






