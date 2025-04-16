Trivy is the image scanner by aqua security. 
using trivy we can scan the image and patch the image with scan vulnerabilities.

1. installing trivy

```
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin v0.61.0
```

2. check installed version
```
trivy --version
```

3. pull image 
```
crictl pull public.ecr.aws/docker/library/python:3.12.4
```

4. scan images
```
trivy image public.ecr.aws/docker/library/python:3.12.4 > /root/python_12.txt
```

5. scan image only for high severity.
```
trivy image --severity HIGH public.ecr.aws/docker/library/python:3.9-bullseye > /root/python.txt
```

6. scan image in tarball json format
```
trivy image --input alpine.tar -f json > /root/alpine.json
```   