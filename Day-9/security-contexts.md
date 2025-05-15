## Security Contexts

We can specify security configurations to Docker Containers using `--user`, `--group-add`, `--cap-add`, and `--cap-drop` options. These options allow us to run containers with specific user IDs, group IDs, and capabilities.<br>

**Example:**
```bash
docker run -d \
  --name my-container \
  --user 1000:1000 \
  --group-add docker \
  --cap-add NET_ADMIN \
  --cap-drop SYS_ADMIN \
  my-image
```

### Security Contexts in Kubernetes

We can get this same functionaluty with little more flexibility using `securityContext` in Kubernetes. This allows us to set security options at the Pod level or at the Container level.

**Example for Container Level:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      securityContext:
        runAsUser: 1000
        runAsGroup: 1000
        capabilities:
          add:
            - NET_ADMIN
          drop:
            - SYS_ADMIN
```

**Example for Pod Level:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 2000
  containers:
    - name: my-container
      image: my-image
      securityContext:
        capabilities:
          add:
            - NET_ADMIN
          drop:
            - SYS_ADMIN
```

If same security context is defined at both Pod and Container level, the container level security context will override the pod level security context.

Date of Commit: 15/05/2025