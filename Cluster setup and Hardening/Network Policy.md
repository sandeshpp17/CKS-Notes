Kubernetes by default allows the communication from all pods to all pods. there is no restriction for communication between pods. 
Network policy play role while allowing or denying the network communication between pods.

let say that we have db pod in namespac1 and other application in namespace2 and webUI or backend in namespace3. If we want to allow the communication from namespace3 to namespace1 for particular db pod then it can be achieved. 

Network Policy apply using the labels. below is the example of network policy for pod which has a label of `role=db`

[`service/networking/networkpolicy.yaml`](https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/service/networking/networkpolicy.yaml) ![](https://kubernetes.io/images/copycode.svg "Copy service/networking/networkpolicy.yaml to clipboard")

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress # This is the from traffic
  - Egress #  This is the to traffic
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978

```

### network policy ingress

let consider we have two pods A and B. if we configure the policy on pod A to allow the traffic form pod B

(Pod B) ----> (Allowed (Pod A) Policy)
(Pod B) ----> (Denied (Pod A) Policy)

if the denied policy is applied then pod B is unable to reach pod A.

below are the example of OR and AND 

OR example
```yaml
	- ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:  # - hypen is used for or 
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
```

AND example
```
	- ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
      namespaceSelector:
        matchLabels:
          project: myproject
      podSelector:
        matchLabels:
          role: frontend
```


https://kubernetes.io/docs/concepts/services-networking/network-policies/