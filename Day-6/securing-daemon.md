## Securing the Docker Daemon

If someone gains access to the Docker daemon, they can access all the containers, volumes, and networks on the host. This is a significant security risk. Also they can lauch a privileged container and gain root access to the host.

### Securing the Docker host

We must secure the Docker host itself. This includes:
- **Disabling Password based authentication**
- **Use SSH Based Authentication**
- **Disable Root Login**
- **Disable Unused Ports**
ans many more.

### Exposing the Docker Daemon

If we need to expose the Docker daemon, It is recommended to use Private Interfaces.

### Using TLS

- If we are using only `--tls` option it is not enough to enable authentication. We need to use `--tlsverify` option to enable authentication.
- This will require clients to present a valid client certificate to connect to the Docker daemon.
- So we will generate one CA cert and pass it in the `daemon.json` file
```{
  "hosts": ["tcp://192.168.1.10:2376"],
  "tls": true,
  "tlscert": "/var/docker/server.pem",
  "tlskey": "/var/docker/serverkey.pem",
  "tlsverify": true,
  "tlscacert": "/var/docker/cacert.pem"
}
```
- Now clients will need to present a valid client certificate to connect to the Docker daemon. We can generate client certs using the CA cert.
- From the client machine(They should have valid Client certs and CA cert), we can use the following command to connect to the Docker daemon:
```bash
export DOCKER_TLS_VERIFY=true
export DOCKER_HOST="tcp://192.168.1.10:2376"
docker --tlscert=<path_to_cert> --tlskey=<path_to_key> --tlscacert=<path_to_ca_cert> ps
``` 
