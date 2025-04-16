Ingress is a kubernetes network resource which help to route the traffic of user to pod/service endpoint.

Whenever a new pod is deployed we access it using the NodePort or Load-Balancer. with the new port its hard to remember each application address. To solve this the ingress resource is needed.

Multiple 3rd party provider are used for deploying ingress resource. 
ex. [NGINX Ingress Controller](https://www.nginx.com/products/nginx-ingress-controller/), [Traefik](https://doc.traefik.io/traefik/providers/kubernetes-ingress/), [HAProxy Ingress](https://haproxy-ingress.github.io/) and [Istio Ingress](https://istio.io/latest/docs/tasks/traffic-management/ingress/kubernetes-ingress/)

below is the step to deploy nginx ingress resource.

- create ingress controller with helm or resource defination.
```shell
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

```shell
helm show values ingress-nginx --repo https://kubernetes.github.io/ingress-nginx
```
- create the ingress controller definition.
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.1/deploy/static/provider/cloud/deploy.yaml
```

- create ingress
```YAML
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world
  annotations:
    kubernetes.io/ingress.class: ingress-nginx
spec:
  rules:
  - host: host1.domain.ext
    http:
      paths:
      - backend:
          serviceName: hello-world
          servicePort: 80
```

