kuebsec is manifest scanning application. which tell the user before running the deployment.

Installing kubesec

```
wget https://github.com/controlplaneio/kubesec/releases/download/v2.14.2/kubesec_linux_amd64.tar.gz
```

```
tar -xvf kubesec_linux_amd64.tar.gz
mv kubesec /usr/bin/
```

scanning the yaml with kubesec

```
kubesec scan ./node.yaml -o json > kubesec_report.json
```


https://kubesec.io/