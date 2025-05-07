## Understanding Kubernetes Attack Surface

### Commands Used to Attack Kubernetes Cluster

- `ping domain.com` - To identify the IP address of the domain.
- `zsh port-scan.sh ip-address` - To scan the ports of the target IP address.
- `docker -H www.xxx.com ps` - To list the running containers on the target Docker host. If the Docker API is exposed to the public, this command can be used to list all the containers running on the target host. Depends upon our use case we can use any other commands in place of `ps` command.
- `docker -H www.xxx.com run --privileged -it ubuntu bash` - To run a privileged container on the target Docker host. This command can be used to gain access to the target host and run commands as root. This is a very powerful command and should be used with caution.
- `hostname` - To get the hostname of the target host. Sometimes the hostname can be used to identify the target host purpose or the application running on it.
- `sudo iptables -L -t nat | grep kubernetes` - To list the NAT rules in the iptables. This command can be used to identify the Kubernetes cluster and the services running on it.

### Vulnerabilities

- **Dirty Cow** - A vulnerability in the Linux kernel that allows an attacker to gain root access to the host system. This vulnerability can be exploited by running a privileged container on the target host.

### Security Tips

- `Docker Ports` Should not be exposed to the public without proper Authentication and Authorization.

Commit Date: 29/04/2025