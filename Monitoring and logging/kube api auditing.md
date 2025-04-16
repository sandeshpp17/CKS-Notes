in the cluster who made what change are not recorded by default. to enable the auditing of user change in the cluster the audit logs are used.

below are step to enable audit in api server.

add below flag in kube api configuration
```yaml
 - --audit-policy-file=/etc/kubernetes/audit-policy.yaml
 - --audit-log-path=/var/log/kubernetes/audit/audit.log
```

also this file should be available in container so mount volume to container.

```yaml
volumeMounts:
  - mountPath: /etc/kubernetes/audit-policy.yaml
    name: audit
    readOnly: true
  - mountPath: /var/log/kubernetes/audit/
    name: audit-log
    readOnly: false
```

and finally configure the `hostPath`:

```yaml
...
volumes:
- name: audit
  hostPath:
    path: /etc/kubernetes/audit-policy.yaml
    type: File

- name: audit-log
  hostPath:
    path: /var/log/kubernetes/audit/
    type: DirectoryOrCreate
```

below are extra flags for audit

- `--audit-log-path` specifies the log file path that log backend uses to write audit events. Not specifying this flag disables log backend. `-` means standard out
- `--audit-log-maxage` defined the maximum number of days to retain old audit log files
- `--audit-log-maxbackup` defines the maximum number of audit log files to retain
- `--audit-log-maxsize` defines the maximum size in megabytes of the audit log file before it gets rotated