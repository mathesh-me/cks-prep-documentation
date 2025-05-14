## Linux System Calls

### Linux Kernel

Linux Kernel acts as an interface between the applications running on the user space and the hardware. Whenever an application needs to perform a task that requires access to the hardware, it makes a system call to the kernel. The kernel then performs the requested operation and returns the result to the application. The kernel space contains the kernel code, device drivers, and its extensions—all essential for proper communication between hardware and applications.

### Tracing System Calls

Using `strace` command, we can trace the system calls made by a process. This is useful for debugging and understanding how a program interacts with the kernel.

- To trace the system calls made by a process, use the following command:
```bash
strace -p <pid>
```
- To trace the system calls made by a command, use the following command:
```bash
strace <command>
```
- To save the output of `strace` to a file, use the following command:
```bash
strace -o <output-file> <command>
```
- To create a summary of the system calls made by a process, use the following command:
```bash
strace -c <command>
```

### System Call Output 

- The output of `strace` shows the system calls made by the process, along with their arguments and return values. Each line of the output represents a single system call.
```
execve("/usr/bin/touch", ["touch", "/tmp/error.log"], 0x7ffce8f874f8 /* 23 vars */) = 0

[Output Truncated]
```
    - **execve**: system cal name
    - **"/usr/bin/touch"**: path to the executable
    - **["touch", "/tmp/error.log"]**: arguments passed to the executable
    - **23 vars**: environment variables passed to the executable
    - **= 0**: return value of the system call (0 indicates success)

Date of Commit: 13/05/2025