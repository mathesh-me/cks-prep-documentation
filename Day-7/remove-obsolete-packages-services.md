## Remove Obsolete Packages and Services

- Some software packages and services might come pre-installed our system. We may don't need them. These package will increase the Complexity of the system and may also increase the attack surface. We should remove any obsolete packages and services that are not needed.

- To remove unwanted packages, we can use the following command:
```bash
apt remove <package-name>
```
- If there is a message saying there are leftover packages you don’t need anymore. Use the below command to remove them:
```
apt autoremove
```

### Managing Services

- Services are managed by `systemd`. To check the status of a service, use the following command:
```bash
systemctl status <service-name>
```
- To stop a service, use the following command:
```bash
systemctl stop <service-name>
```
- List all running services:
```bash
systemctl list-units --type=service
```
- To disable a service from starting at boot time, use the following command:
```bash
systemctl stop <service-name>
systemctl disable <service-name>
```
- We can also Enable/disable services from starting on boot.

Date of Commit: 13/05/2025