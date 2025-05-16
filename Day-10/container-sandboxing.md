## Container Sandboxing

### VMs vs Containers

- VMs running on a hypervisor will have their own kernel and OS, This allows for a higher level of isolation.
- Containers running on a host OS share the same kernel and OS, which can lead to security issues if not properly configured.

### Process ID Namespace

- Processes running on a container will have their own PID inside container and also it will another PID in the host.
- Terminating a process on the host will terminate the process inside the container.
- Applications or Processes running inside a container will be consider just like a applications or processes running on the host that will execute in user space and access hardware resources through system calls. This shared kernel approach is not secure and may lead to security risks.

### Sandboxing techniques for Containers

Sandboxing techniques are used to isolate containers from each other and from the host system. This is important for security and resource management.

Some of the techniques referred till now are:
- AppArmor
- Seccomp

Date of Commit: 16/05/2025