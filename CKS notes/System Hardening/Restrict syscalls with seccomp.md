seccomp is use to restrict the system call in kubernetes the path of seccomp profile is `/var/lib/kubelet/seccomp/`

you will be given the seccomp profile copy it k8s seccomp path and enable it by passing value in pod manifest.

below is example of seccomp profile.
[`pods/security/seccomp/profiles/audit.json`](https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/pods/security/seccomp/profiles/audit.json) ![](https://kubernetes.io/images/copycode.svg "Copy pods/security/seccomp/profiles/audit.json to clipboard")

```json
{
    "defaultAction": "SCMP_ACT_LOG"
}
```

Below is example of pod using seccomp profile.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: audit-pod
  labels:
    app: audit-pod
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
  containers:
  - name: test-container
    image: hashicorp/http-echo:1.0
    args:
    - "-text=just made some syscalls!"
    securityContext:
      allowPrivilegeEscalation: false
```

below link for k8s seccomps 
https://kubernetes.io/docs/tutorials/security/seccomp/