## Limit Node Access

### Limit Node Access

- Both the Control Plane and Worker nodes should not be exposed to the internet.
- If using a cloud provider, We can't access Control Plane nodes, but at least we should make sure that worker nodes are provisioned in a self-hosted or private network. Access to the worker nodes should be done by using VPC or using firewall rules to only allow certain IPs from our Private Network.

### User Accounts in Linux

- **User accounts** - Normal users who can log in to the system and perform tasks.
- **Superuser** - A user(UID 0) with administrative privileges who can perform any task on the system.
- **System accounts** - Accounts created by the system for specific purposes, such as running services or daemons.  It will by software applications. These accounts typically have limited privileges and are not intended for interactive use.
- **Service accounts** - Accounts used by services to authenticate and authorize access to resources. 

### Inspecting User Information

- **id** - Displays user and group IDs.
- **who** - Displays information about users currently logged in.
- **last** - Displays a list of last logged-in users.

### Access Control files

- **/etc/passwd** - *Contains user account information, including usernames, UIDs, GIDs, home directories, and default shells.*
- **/etc/shadow** - *Contains encrypted passwords and password expiration information.*
- **/etc/group** - *Contains group account information, including group names, GIDs, and member users.*

### Working with User Accounts

- **usermod -s /bin/nologin <username>** - *Disables login for a user by changing the shell to /bin/nologin.*
- **deluser <username> group** - *Removes a user from a group.*

Date of Commit: 12/05/2025