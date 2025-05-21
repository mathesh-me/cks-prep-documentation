## Ensuring Immutability of Containers

### Ways to Ensure Immutability

#### Read-Only File System

- By default, containers are created with a read-write file system. This means that we can modify the files in the container. If we need to ensure that the files in the container are not modified, we can create the container with a read-only file system. This will prevent any modifications to the files in the container.

- We can create a container with a read-only file system by using the `readOnlyRootFilesystem` option in the Pod security context. This will make the root file system of the container read-only. Any attempts to modify the files in the container will result in an error.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: myimage
    securityContext:
      readOnlyRootFilesystem: true
```

This will create a Pod with a container that has a read-only file system. Any attempts to modify the files in the container will result in an error.

- Sometimes the above approach might not work as expected. For example, if we are running a application that needs to write to a file, we can create a emptyDir volume and mount it to the container. This will allow the application to write to the file in the emptyDir volume, but the root file system of the container will still be read-only.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: myimage
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: myvolume
      mountPath: /data
  volumes:
  - name: myvolume
    emptyDir: {}
```
This will create a Pod with a container that has a read-only file system and a emptyDir volume mounted to the `/data` directory. The application can write to the files in the `/data` directory, but the root file system of the container will still be read-only.

#### Avoiding `Privileged` Containers

- When we set the `privileged` flag to true, the container will have access to do many things that a normal container cannot do. This includes modifying the file system, accessing the host network, and accessing the host file system. This can lead to security vulnerabilities if the container is compromised.
- If we are using `readOnlyRootFilesystem` in the Pod security context with `privileged` containers, it will still block the modifications to the root file system of the container. But it will not block the modifications to the other file systems like pseudo file systems like `/proc` and `/sys` like modifying the swappiness value can impact host system.\

### Best Practices for Ensuring Immutability

- Use `readOnlyRootFilesystem` in the Pod security context to ensure that the root file system of the container is read-only.
- Limited Write access to the container by using `emptyDir` volumes for temporary files.
- Avoid using `privileged` containers unless absolutely necessary.
- Use `runAsNonRoot` in the container security context to ensure that the container is not running as root. This will prevent any modifications to the files in the container.
- Implementing Pod Security Policies to enforce the above security context settings.

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: example
spec:
  privileged: false
  readOnlyRootFilesystem: true
  runAsUser:
    rule: RunAsNonRoot
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
```

Date of Commit: 21/05/2025