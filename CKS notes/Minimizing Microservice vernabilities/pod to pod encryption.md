## pod to pod encryption with cilium

cilium uses the eBPF for encrypting pods. as we seen with istio we need to create the new side car container. but cilium does it background without changing code of application.

1. To install cilium your cluster with cilium command

```
cilium install --version 1.17.2    --set encryption.enabled=true    --set encryption.type=wireguard
```

below is with helm
```
helm install cilium cilium/cilium --version 1.17.2 \
  --namespace kube-system \
  --set encryption.enabled=true \
  --set encryption.type=wireguard
```

To verify create 2 pod 1 is nginx and other is curl.

use curl command in curl container to nginx.

```
kubectl -n kube-system exec -ti ds/cilium -- bash
```

2. Check that WireGuard has been enabled (number of peers should correspond to a number of nodes subtracted by one):
```
cilium-dbg status | grep Encryption
``` 

Encryption: Wireguard [cilium_wg0 (Pubkey: <..>, Port: 51871, Peers: 2)]

3. Install tcpdump

```
apt-get update
apt-get -y install tcpdump    
``` 

4. Check that traffic is sent via the `cilium_wg0` tunnel device:
```
tcpdump -n -i cilium_wg0
```

