## Restricting System Call for Processes and Applications using Seccomp

By default, Linux allows all system calls to be executed by any process. This can be a security risk, as some syscalls can be exploied by attackers to perform malicious actions. 

### Seccomp

- Seccomp (Secure Computing Mode) is a Linux kernel feature that allows us to restrict the system calls that a process or application can make. It provides a way to filter and limit the system calls that a process can use, reducing the attack surface and improving security.<br>
- To verify if seccomp is enabled on your system, you can check the kernel configuration. You can do this by running the following command:
```bash
cat /boot/config-$(uname -r) | grep CONFIG_SECCOMP

# If seccomp is enabled, you should see the following output:
# CONFIG_SECCOMP=y
```
- There are three modes of seccomp:
    - **Mode 0**: Seccomp disabled. All system calls are allowed.
    - **Mode 1**: Strict mode. Only a limited set of system calls are allowed.(e.g., read, write, exit, sigreturn)
    - **Mode 2**: Filter mode. Allows syscalls based on a filtering profile.

### Docker Default Seccomp Profile

- Docker automatically applies a default seccomp profile to all containers if seccomp is enabled on the host. The default profile approximately whitelists 60 syscalls.<br>

**Default Profile**:

```json
{
  "defaultAction": "SCMP_ACT_ERRNO", // For syscalls that are not explicitly specified in the syscalls list
  "architectures": [ // Supported architectures
    "SCMP_ARCH_X86_64",
    "SCMP_ARCH_X86",
    "SCMP_ARCH_X32"
  ],
  "syscalls": [ // syscalls list
    {
      "names": [
        "arch_prctl",
        "brk",
        "capget",
        "capset",
        "mkdir",
        "close",
        "execve",
        "...",
        "clone"
      ],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```
In the above example, the default action is to return an error (SCMP_ACT_ERRNO) for any syscall not explicitly allowed. The syscalls listed under "names" are allowed, while all others are denied.<br>

- We can also create a custom seccomp profile to allow or deny specific syscalls based on our requirements. This can be done by creating a JSON file with the desired seccomp rules and then passing the path of file to the Docker run command using the `--security-opt` flag.<br>

- To disable seccomp for a container, we can use the `--security-opt seccomp=unconfined` option when running the container. This will allow all syscalls to be executed within the container.<br>

> Even without a seccomp profile, some of the syscalls may remain blocked by additional Docker security measures.

Date of Commit: 14/05/2025
