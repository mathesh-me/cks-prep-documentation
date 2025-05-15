## Admission Controllers

When we apply `kubectl` commands to create or modify resources in a Kubernetes cluster, the request will be sent to the API server. The API server will then process the request which involves authentication, authorization, admission control, and finally persistence of the resource in etcd.

### Admission Controllers

Once Authentication and Authorization are successful, the request is sent to the Admission Controllers. Admission controllers helps us to have a control over the resources that are being created or modified in the cluster. They can be used to enforce policies, validate requests, and modify requests before they are persisted in etcd.<br>

**Example Scenarios:**

- Reject the request if the container image is not from a trusted registry.
- Modify the container's security context to run as a non-root user.

**Flow of Request:**

`kubectl` command -> Authentication -> Authorization -> Admission Controllers -> Create/Update Resource in etcd

**Built-in Admission Controllers:**

Some of the built-in admission controllers are:

- `AlwaysPullImages`: Always pull the image from the registry before creating a Pod.
- `DefaultStorageClass`: Automatically assign a default StorageClass to PersistentVolumeClaims if none is specified.
- `NamespaceExists`: Reject requests for namespaces that do not exist.
- `PodSecurityPolicy`: Enforce security policies for Pods.

### Configuring Admission Controllers

- Enabling Admission Controllers: Admission controllers can be enabled or disabled by modifying the `--enable-admission-plugins` flag in the API server configuration. **Example:** `--enable-admission-plugins=NodeRestriction,NamespaceAutoProvision`
- For disabling admission controllers, we can use the `--disable-admission-plugins` flag.

Date of Commit: 15/05/2025