Type of sbom are SPDX and CycloneDX.

SBOM is know as software bill of materials. 


To generate the sbom install syft

```shell
curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin
```

to generate the sbom in spdx format
```
syft scan docker.io/kodekloud/webapp-color:latest -o spdx  > /root/webapp-spdx.sbom
```

To generate sbom in cyclonedx json format
```
syft docker.io/kodekloud/webapp-color:latest -o cyclonedx-json > /root/webapp-sbom.json
```

### Scan vulnerability sbom using grype

installing grype
```
curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b /usr/local/bin
```

to generate of vulnerability report.
```
grype sbom:webapp-spdx.sbom -o json > grype-report.json
```

