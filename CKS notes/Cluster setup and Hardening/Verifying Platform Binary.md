When the kubernetes package get release with new version it always has a check sum value with binary. Its useful for checking that we are using legitimate package in our cluster.

Go to the k8s official release page and check for the version that you have installed in you system.

follow this [link](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.32.md) for verifying the checksum.

for verification use sha512sum command.

```shell
sha512sum <package-path>
```

below are the step for verification.

- Download the latest release with the command:

  - [x86-64](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#download-convert-binary-linux-0)
  - [ARM64](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#download-convert-binary-linux-1)
  
```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"    
```  

- Validate the binary (optional)
  
  Download the kubectl-convert checksum file:
  
  - [x86-64](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#download-convert-checksum-linux-0)
  - [ARM64](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#download-convert-checksum-linux-1)
 ```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
```
  
  Validate the kubectl-convert binary against the checksum file:
  
```bash
   echo "$(cat kubectl-convert.sha256) kubectl-convert" | sha256sum --check
```
  
If valid, the output is:
 
```console
   kubectl-convert: OK
```
  
  If the check fails, `sha256` exits with nonzero status and prints output similar to:  
```console
kubectl-convert: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```