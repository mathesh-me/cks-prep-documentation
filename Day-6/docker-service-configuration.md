## Docker Service Configuration

When we run Docker as a service, it runs in the background and manages containers for us. It will automatically start when the system boots up. But we can also use `dockerd` command to start the Docker daemon manually in the foreground. This is useful for debugging or when we want to run Docker in a non-service mode.

### Options for Docker Daemon

When we are using `dockerd` command, we can pass various options to configure the Docker daemon. Some of the common options are:

- `--debug`: Enable debug mode. This will provide more detailed logs and is useful for troubleshooting.
- `--host`: By default, Docker listens on a Unix socket `/var/run/docker.sock`. This allows `Docker CLI` to communicate with the Docker daemon. If we want other clients to connect to the Docker daemon, we can specify a TCP socket Interface using this option. For example, `--host tcp://<ip>:2375`.
    - This allows other clients to connect to the Docker daemon. Clinets just need to specify the ENV variable `DOCKER_HOST` to point to the TCP socket. Example: `export DOCKER_HOST="tcp://192.168.1.10:2375"`
    - Also we need to make sure that the interface we are exposing is not accessible from the public internet. This is a security risk. Private interfaces are preferred. When we are exposing the Docker daemon, we should also consider using TLS to encrypt the connection. This is important for security. By default, Docker does not use TLS for the TCP socket. So, we need to generate Server certs and use them as flags for `dockerd` command.
    - **Example Command:**
        ```bash
        dockerd --debug \
        --host=tcp://192.168.1.10:2376 \ # when we use tls, the port should be 2376
        --tls=true \
        --tlscert=/var/docker/server.pem \
        --tlskey=/var/docker/serverkey.pem
        ```
    - This configurations can also be done using the `daemon.json` file. This file is located at `/etc/docker/daemon.json`. We can add the following configuration to the file:
        ```json
        {
        "debug": true,
        "hosts": ["tcp://192.168.1.10:2376"],
        "tls": true,
        "tlscert": "/var/docker/server.pem",
        "tlskey": "/var/docker/serverkey.pem"
        }
        ```
        - Once the configuration is done, we need to restart the Docker daemon for the changes to take effect. We can do this using the following command:
        ```bash
        sudo systemctl restart docker
        ```

Date of Commit: 12/05/2025