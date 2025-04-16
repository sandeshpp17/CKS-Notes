Using apparmor we can restrict the system call and restriction any action to perform. we use `aa status` to check the status of app armor profile and `aa parse -r or -f` to enable the app armor profile.

Alternate commands: `apparmor_status and apparmor_parse`

below is example of app armor profile
```
#include <tunables/global>

profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>

  file,

  # Deny all file writes.
  deny /** w,
}
```

using below pod example we can enable the app armor profile.
check the name of profile it should match with localhostprofile.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: k8s-apparmor-example-deny-write
  containers:
  - name: hello
    image: busybox:1.28
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

