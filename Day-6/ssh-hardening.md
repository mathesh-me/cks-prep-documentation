## SSH Hardening

### SSH Process

- SSH Service should be installed on the system which we want to access and Port 22 should be open.
- From the client machine, User will generate a key pair using the command `ssh-keygen -t rsa` command.
- The public key will be copied to the server using the command `ssh-copy-id <username>@<server-ip>`.
- The private key will be stored in the `~/.ssh/` directory of the client machine.
- The user can now log in to the server using the command `ssh <username>@<server-ip>`.
- The SSH service will check the public key in the `~/.ssh/authorized_keys` file of the server and if it matches with the private key of the client machine, then the user will be logged in to the server.

### SSH Hardening
- **Disable Root Login**: Prevent direct root login to enhance security.
  - Edit `/etc/ssh/sshd_config` and set `PermitRootLogin no`.
- **Disable Password Authentication**: Use key-based authentication instead of passwords.
  - Edit `/etc/ssh/sshd_config` and set `PasswordAuthentication no`.

Once the changes are made, restart the SSH service using the command `systemctl restart sshd`.

Date of Commit: 12/05/2025