To restrict the dns resoultion of cross namespace. and resource can only be found in same name space we can add the CORE DNS rule to disable cross namespace dns resolution.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
data:
  Corefile: |
    .:53 {
        errors
        health {
            lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
            pods verified
            fallthrough in-namespace
            
        }
        prometheus :9153
        forward . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }    
```

