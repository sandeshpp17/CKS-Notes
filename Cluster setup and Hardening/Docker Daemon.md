If the docker daemon port open on the firewall of host and anyone can access it. then anyone can run there container on your server using it for mining bit coins and running there own application.
The port for docker daemon is 2376.

By setting the environment variable as below the user can access the docker daemon.

```
export DOCKER_HOST="tcp://server-ip:2376"
```

pass this variable in /etc/docker/daemon.json if you don't want pass env variable.

## Securing docker daemon

To secure the docker daemon you have to enable authentication with TLS.

below is the command to enable to TLS.
```console
dockerd \
    --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=server-cert.pem \
    --tlskey=server-key.pem \
    -H=0.0.0.0:2376
```

tls=true is the flag to pass tls without verification.
tlsverify=true is the flag for verification of tls.

```JSON
{
	"host": ["tcp://serverip:2376"]
	"tlscert": "/path/to/tls/cert"
	"tlskey": "/path/to/tls/key"
	"tlsverify": true
	"tlscacert": "/path/to/tls/cacert"
}
```

you can pass the tls without specifying in json file. just put the tls in ~/.docker/ path so docker can put-up this tls certs.

Follow below link for securing docker daemon.
https://docs.docker.com/engine/security/protect-access/


