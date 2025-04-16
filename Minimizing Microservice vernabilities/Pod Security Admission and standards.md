In term of securing pod we implement the pod security policy. but due to deprecation of pod security. The pod security admission and pod security standards are came in picture.

using the pod security admission and standards we can enforce the pod in security standards. To enable the pod security both admission and standards are used by labeling the name space with different combination.

below is pod security standards

| Profile        | Description                                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Privileged** | Unrestricted policy, providing the widest possible level of permissions. This policy allows for known privilege escalations.         |
| **Baseline**   | Minimally restrictive policy which prevents known privilege escalations. Allows the default (minimally specified) Pod configuration. |
| **Restricted** | Heavily restricted policy, following current Pod hardening best practices.                                                           |
pair it with pod security admission.

| Mode        | Description                                                                                                                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **enforce** | Policy violations will cause the pod to be rejected.                                                                                                                                                 |
| **audit**   | Policy violations will trigger the addition of an audit annotation to the event recorded in the [audit log](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/), but are otherwise allowed. |
| **warn**    | Policy violations will trigger a user-facing warning, but are otherwise allowed.                                                                                                                     |

To enable pod security on namespace.

```shell
kubectl label --overwrite ns my-existing-namespace \
  pod-security.kubernetes.io/enforce=restricted \
  pod-security.kubernetes.io/enforce-version=v1.32
```
