Opa is authentication and authorization agent which verify and allows access to the application.

to install the opa check the releases page.
https://github.com/open-policy-agent/opa/releases

wget https://github.com/open-policy-agent/opa/releases/download/$(version)/opa_linux_amd64

To run the opa as a server.

./opa run -s &

To add policy in opa use below sample.rego should be present.

curl -X PUT --data-binary @sample.rego http://localhost:8181/v1/policies/samplepolicy


## OPA with kubernetes.

To integrate opa with kubernetes you have to create the deployment and intergrate it with kube api server as vaildting-webhook.

Once its deployed. then created the configmap with opa .rego file.


### Constraint Templates and Constraints

using opa constraint we can apply enforce opa policy. 

constraint template:
		its the template of opa constraints where the rule is enforced and constraint help to imply that rule.

```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels # -> this is used for kind of constraint
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          type: object
          properties:
            labels:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels

        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("you must provide labels: %v", [missing])
        }
```


Constraints: To imply the template the constraints are used keep in mind the kind which  is used in crd will be used in constraint kind.

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels # <- this is taken from template crd
metadata:
  name: ns-must-have-gk
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters:
    labels: ["gatekeeper"]
```

below is example of constraint which is restrict the creation of pod if label is not present in creation.

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: matchlabel
spec:
  namespaces: ["testnamespace"]
  parameters:
    labels: ["tech"]
```


Below is constraints for replicas.

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: matchlabel
spec:
  match:
    kinds:
      - apiGroups: ["apps"]
        kinds: ["Deployment"]
  parameters:
    ranges:
    - min_replicas= 2
      max_replicas= 5
```