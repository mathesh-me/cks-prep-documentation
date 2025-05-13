## UFW 

- UFW (Uncomplicated Firewall) is a user-friendly interface for managing iptables firewall rules. It is designed to make it easier to configure a firewall on Linux systems. UFW use Netfilter(Linux kernel's internal packet filtering system) behind the scenes to manage the firewall rules. Usually IPtables is used to manage the firewall rules. UFW is a front-end for iptables and provides a simpler way to manage firewall rules.

### UFW Commands

- To check the status of UFW:
```bash
ufw status
```
- To enable UFW:
```bash
ufw enable
```
- To disable UFW:
```bash
ufw disable
```
- To allow a specific port (e.g., port 22 for SSH):
```bash
ufw allow 22
```
- To allow a specific IP on a specific port:
```bash
ufw allow from <ip-address> to any port <port-number>
```
- To deny a specific port:
```bash
ufw deny <port-number>
```
- To delete a rule:
```bash
ufw delete allow <port-number>
```
- To delete a rule by number:
```bash
ufw delete <rule-number>
```
- To check the UFW logs:
```bash
ufw status verbose
```
- To reset UFW to its default state:
```bash
ufw reset
```

Date of Commit: 13/05/2025