## Linux Capabilities

Even if seccomp profile is set to unconfined, the container will still be limited by the Linux capabilities. 

### Linux Privilege Process

**Before Linux 2.4**: The Linux kernel had two type of privilege processes:
- **Root(Privileged) Processes**: Root processes had full access to all system resources and could perform any action on the system.
- **Non-root(Unprivileged) Processes**: Non-root processes had limited access to system resources and could only perform actions that were allowed by the system administrator.

**After Linux 2.4**: The Linux kernel introduced a new security model called "Linux capabilities" that allows us to split the privileges of the root user into smaller, more manageable pieces. This allows us to grant specific privileges to processes without giving them full root access. Examples: CAP_SYS_ADMIN, CAP_NET_ADMIN, CAP_DAC_OVERRIDE, etc.

### Checking Capabilities

- To check the capabilties required for a Command:
```bash
getcap /usr/bin/ping
```
- To check the capabilities of a process:
```bash
getcaps pid
```

### Modiying Container Capabilities

- To add or remove capabilities from a container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
spec:
    containers:
        - name: ubuntu-sleeper
        image: ubuntu
        command: [ "sh", "-c", "echo 'Sleeping for an hour!' && sleep 1h" ]
        securityContext:
            capabilities:
            add:
                - NET_ADMIN
                - SYS_ADMIN
            drop:
                - NET_ADMIN
```

Date of Commit: 14/05/2025