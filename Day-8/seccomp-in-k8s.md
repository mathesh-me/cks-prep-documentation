## Seccomp in Kubernetes

### Default Seccomp Profile in Docker

- We can check the default seccomp profile used by Docker by using a container introspection tool called amicontained.

```bash
docker run r.j3ss.co/amicontained amicontained
```
The command output will show details about seccomp configuration used by Docker.

- We can run this `amicontained` command on k8s pod as well
```bash
kubectl run amicontained --image=r.j3ss.co/amicontained amicontained -- amicontained
```
Then inspect the pod logs `kubectl logs amicontained` to see the seccomp profile used by the pod.

### Enabling Seccomp in Kubernetes Pod

- Using default runtime Profile
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: amicontained
  name: amicontained
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault # If we are specifying unconfined, it will run a pod without any seccomp restrictions.
  containers:
    - args:
        - amicontained
      image: r.j3ss.co/amicontained
      name: amicontained
      securityContext:
        allowPrivilegeEscalation: false
```

- Using custom seccomp profile
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-audit
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: <path to the custom JSON file> # This path is relative to the /var/lib/kubelet/seccomp/ directory on the node.
  containers:
    - command: ["bash", "-c", "echo 'I just made some syscalls' && sleep 100"]
      image: ubuntu
      name: ubuntu
      securityContext:
        allowPrivilegeEscalation: false
```

### Logging Syscalls in Containers

- Create a Custom Seccomp Profile and specify the file location in the pod spec.
```json
{
  "defaultAction": "SCMP_ACT_LOG"
}
```
Now we can see the logs of containers in the `/var/log/syslog` file on the host machine. The logs will contain information about the syscalls made by the container, including the syscall name, arguments, and return value.

### Seccomp file to Reject all syscalls in Pod

```json
{
  "defaultAction": "SCMP_ACT_ERRNO"
}
```

> Analyze the syscalls required for your application by using audit logs or by using tracee to create a custom seccomp profile.

Date of Commit: 14/05/2025