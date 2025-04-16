Falco is used for alerting if anything wrong happens in the cluster.

Falco uses the eBPF to check the system call in kernel. falco needs to be installed as the package and installing package by default installs the falco kernel module.

To check the falco is running use

```
systemctl status falco
```

- falco default configuration file is located at `/etc/falco/falco.yaml`
- new files can be use for writing falco rule for triggering alert the order of these file important if you same rule in different file the last file rule will be applied.
- in default falco configuration the logs are not enable to redirect the logs at particular path use `file_output` option.

below is the example of rule.
```yaml
rule: shell_in_container
  desc: notice shell activity within a container
  condition: >
    evt.type = execve and 
    evt.dir = < and 
    container.id != host and 
    (proc.name = bash or
     proc.name = ksh)    
  output: >
    shell in a container
    (user=%user.name container_id=%container.id container_name=%container.name 
    shell=%proc.name parent=%proc.pname cmdline=%proc.cmdline)    
  priority: WARNING
```


To apply the rule restart the service or kill the falco pid.
```
systemctl restart falco
```