## gVisor 

`gVisor` acts as a isolation layer between applications running in containers and the host kernel. When a container makes a system call, it is intercepted by gVisor and processed by gVisor before it reaches the host kernel.

### gVisor Architecture

- **Sentry**: It functions as a lightweight, application-level kernel specifically designed for container workloads. It intercepts system calls made by the containerized application and process them. It's support limited to fewer system calls compared to the host kernel thus providing a more secure environment.
- **Gofer**: When a containerized application makes a system call, the Sentry intercepts it and forwards it to the Gofer(acts as a file proxy). The Gofer is responsible for handling necessary logic for accessing file systems for containerizd applications.

For network related operations, gVisor utilizes it's own network stack for providing isolation.

### Benefits of gVisor

- Every Conatiner with gVisor has its own kernel, which means that if a container is compromised, the attacker will not have access to the host kernel. All the other containers running on the host are also safe.

> Some applications will not be fully compatible with gVisor. Also processing system calls through a middleman may introduce a slight performance overhead.

Date of Commit: 17/05/2025