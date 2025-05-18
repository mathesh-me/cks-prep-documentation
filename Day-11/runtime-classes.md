## Runtime Classes

Container runtime is responsible for running containers. `runc` is the default container runtime for Docker. It also implements standards set by Open Container Initiative (OCI). The OCI runtime spec defines how to run a container. 

### Container Runtimes

Like `runc`, there are other container runtimes too. Some of them are:

- **kata containers**: It have it's own container runtime to provide isolation and security by using lightweight virtual machines (VMs) for each container.
- **gVisor**: gVisor have it's own container runtime called `runsc` to provide container sandboxing.

### Utilizing Different Container Runtimes

we can specify Docker to use a different container runtime for a container using `--runtime` flag. For example, to use `gVisor` runtime, we can run the following command:

```bash
docker run --runtime=runsc <image>
```

### Runtime Classes in Kubernetes

To use different container runtimes in Kubernetes, we can use `RuntimeClass` resource. It allows us to define different runtimes and specify which runtime to use for a pod.

### Example of RuntimeClass

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: gvisor-pod
spec:
  runtimeClassName: gvisor
  containers:
  - name: gvisor-container
    image: nginx
```

- We can verify our container is isolated from the host kernel by running the following command:

```bash
pgrep -a nginx
```

Date of Commit: 17/05/2025