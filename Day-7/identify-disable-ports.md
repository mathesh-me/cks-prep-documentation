## Identify and Disable Open Ports

### Identify Open Ports

- Use the command `netstat -tuln` to list all open ports and their associated services.

### Determin Port Usage

- Use the command `lsof -i :<port-number>` to identify which process is using a specific port.
- Also we can look at the `/etc/services` file to see the list of services and their associated ports.

### Disable Unused Ports

- After identifying the unused ports, we can disable them by either stopping the associated service or using a firewall to block access to the port.
```bash
# Stop a service
systemctl stop <service-name>
systemctl disable <service-name>
```
- To block a port using `iptables`, use the following command:
```bash
iptables -A INPUT -p tcp --dport <port-number> -j DROP
```

Date of Commit: 13/05/2025