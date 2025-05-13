## Privilege Escalation

### Sudo

- Users can use `sudo` to run commands as root user. To check if a user has sudo privileges, run the following command:

```bash
sudo -l
```
- To use sudo, User name must be in the sudoers file. To check if a user is in the sudoers file, run the following command:

```bash
sudo -l -U <username>
```

### Sudoers File

- Sudoers file is loacted at `/etc/sudoers`. It helps to control who can run what commands as root user. To view the sudoers file, run the following command:

```bash
sudo cat /etc/sudoers
```

**Example:**
```bash
# This file MUST be edited with the 'visudo' command as
root    ALL=(ALL:ALL) ALL
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
# Allow mark to run any command
mark    ALL=(ALL:ALL) ALL
# Allow Sarah to reboot the system
sarah localhost=/usr/bin/shutdown -r now
# See sudoers(5) for more information on "#include" directives
#include /etc/sudoers.d
```
- File Syntax:
    - User or Group: The first field identifies the user or group that is assigned privileges. Groups are indicated by a % prefix.
    - Host Specification: The second field defines the host(s) where the privileges apply. Setting this to ALL generally means the rule is valid on any host, though it is often limited to localhost.
    - Run-as Specification: The third field, placed in parentheses, specifies which user(s) the command can be run as. Using ALL allows execution as any user.
    - Command Specification: The final field lists the commands the user is allowed to run. ALL gives access to any command, but you can list specific commands for finer control—like in Sarah's entry, where only certain commands are permitted.

- `sudo` also heps user to run the commands in their own shell rather than swithcing to the root shell.

Date of Commit: 13/05/2025