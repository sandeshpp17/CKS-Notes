Kuberlinuter recommends the change in the yaml file. 
If you have created the pod or deployment manifest file. it can suggest the probes, replicas, security context,non-latest image tag, resource requests/limts.

### Installing kubelinter
```
wget  https://github.com/stackrox/kube-linter/releases/download/v0.7.2/kube-linter-linux.tar.gz
```

```
tar -xvf kube-linter-linux.tar.gz 
 cp kube-linter /usr/local/bin/
```

### analyzing the pod yaml for suggestion.
```
kube-linter lint nginx.yml  > analyze
```

updated pod file after suggestion.
```
controlplane ~ ➜  cat nginx.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - nginx
            topologyKey: "kubernetes.io/hostname"
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        securityContext:
          readOnlyRootFilesystem: true
          runAsUser: 1000
          runAsNonRoot: true
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```