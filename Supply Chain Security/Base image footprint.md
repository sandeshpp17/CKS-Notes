1. use the minimal image that don't contain the unnecessary packages.
2. download the image form official image repository.
3. don't store sensitive data or any data in image or container. use volumes or secret/configmap to store them.
4. remove unnecessary package like curl,wget or notepad.
5. use different images for dev and prod environment.
6. use multi stage build of production environment.
