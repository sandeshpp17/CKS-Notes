Limiting node access contains the restriction of user, changing default ssh port and disabling the root user login. 

## User restriction.
create a user which has sudo access instead of using root user. To disable or stop login of user change shell in `/etc/passwd` file to `/usr/sbin/nologin`. which restrict the user from login.

## Change to ssh port and disable root login.

To change the ssh port edit the `/etc/ssh/sshd_config` and add below flag.

```
Port <your port> # the default port is 22 change it to other port
```

To disable root login edit same file `/etc/ssh/sshd_config` and comment the permitrootlogin or set it to no.

```
# PermitRootLogin yes # comment it
```


## restricting node access on network firewall

Check which service is running on the server. if you found the unnecessary service in your system then stop it.

To check listening port in your system use below command.
```
ss -tunlp
or
netstat -tunlp | grep <service port>
```

restricting with ufw (uncomplicated firewall)

To check the status of firewall.
```
ufw status
```

To allow the communication
```
ufw allow from <iprange> to any port <port> proto tcp
```

To disable all listening on port 
```
ufw deny <port>
```

enabling ufw
```
ufw enable
```