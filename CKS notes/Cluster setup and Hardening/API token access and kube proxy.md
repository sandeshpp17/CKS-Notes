
you can access the api server via kube proxy

to start kubeproxy

```
kubectl proxy --port <port is option by default it start on 8001>
```

$TOKEN is token id and token secret that can be found in kube-system bootstrap secret.

$APISERVER can by found in context 

kubectl config view in clusters cluster server 

use export to pass as env variable

```
export $APISERVER=$(kubectl config view -o jsonpath="{.clusters[0].cluster.server}")
```

```
curl -X GET $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
```

# Kube proxy accessing api server

To access the api server using the curl command you cannot do it directly you have to pass the cert,key and ca-cert to access it.

```shell
curl http://kube-apiserver:6443 -k 
		--key admin.key
		--cert admin.crt
		--cacert ca.crt
```

but if you enable to kube proxy then no need to provide the certs it will you cert in kube config.

