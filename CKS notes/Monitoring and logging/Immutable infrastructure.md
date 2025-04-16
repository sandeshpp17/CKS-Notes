Container immutability is know as once the container is created then updating the container or changing file is not permitted.
the container will only change when the new image is created.

To ensure the immutability use the `readOnlyRootFilesystem: true` flag in the pod. also disable the privileged mode using flag `privileged: false`.

when readonlyrootfilesystem is enable container is unable to run because to run the application in container the app should be able to write is certain directory. to avoid it create volume and volumemount as empty directory in pod manifest.


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    supplementalGroups: [4000]
  volumes:
  - name: sec-ctx-vol
    emptyDir: {}
  containers:
  - name: sec-ctx-demo
    image: busybox:1.28
    securityContext: 
	  readOnlyRootFilesystem: true
    command: [ "sh", "-c", "sleep 1h" ]
    volumeMounts:
    - name: sec-ctx-vol
      mountPath: /data/demo
```

