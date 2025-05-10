## Kubelet Security

### Kubelet

The `Kubelet` is responsible for managing the wokkload on each node in the cluster. It is responsible for registering the node with the API server, managing the pods on the node, and reporting the status of the node and its pods back to the API server. The Kubelet is a critical component of the Kubernetes architecture and is responsible for ensuring that the desired state of the cluster is maintained.

### Kubelet Configuration File

##### Before V1.10

All the Kubelet configuration was done through the command line flags in the Kubelet service file. This was not very flexible and made it difficult to manage the Kubelet configuration.

```service
[Unit]
Description=kubelet


[Service]
ExecStart=/usr/local/bin/kubelet \\
  --container-runtime=remote \\
  --image-pull-progress-deadline=2m \\
  --kubeconfig=/var/lib/kubelet/kubeconfig \\
  --network-plugin=cni \\
  --register-node=true \\
  -v=2 \\
  --cluster-domain=cluster.local \\
  --file-check-frequency=0s \\
  --healthz-port=10248 \\
  --cluster-dns=10.96.0.10 \\
  --http-check-frequency=0s \\
  --sync-frequency=0s
```

##### V1.10 and Later

- Starting with Kubernetes v1.10, the Kubelet configuration can be done through a configuration file(`yaml`). This allows for a more flexible and manageable way to configure the Kubelet.<br>

- The configuration file is located at `/var/lib/kubelet/config.yaml` by default.

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
clusterDomain: cluster.local
fileCheckFrequency: 0s
healthzPort: 10248
clusterDNS:
  - 10.96.0.10
httpCheckFrequency: 0s
syncFrequency: 0s
```

Once the configuration file is created, the Kubelet can be started with the `--config` flag to specify the configuration file. If the same options are specified in both the command line and the configuration file, the command line options will take precedence over the configuration file options.<br>

We can find the Kubelet configuration file location by running the following command:

```bash
ps -ef | grep kubelet
```

### Kubelet Security

Kubelet serves on two ports:
- `10250`: Kubelet to serve the authenticated and authorized API over HTTPS. Used by the API server to communicate with the Kubelet. Will have full API access to everything in the node.
- `10255`: Used to expose a read-only, unauthenticated HTTP endpoint. Because of the lack of auth, this port is deprecated and disabled by default in recent Kubernetes versions.

By default, Anonymous access is allowed on port `10250`. This means that anyone can access the Kubelet API without authentication.

#### Authentication

By default, the flag `--anonymous-auth` is set to `true`, which allows anonymous requests to the Kubelet API. This should be set to `false` to disable anonymous access.

- We can disable anonymous access either by setting the `--anonymous-auth` flag to `false` in the Kubelet Service file or we can set it in the Kubelet configuration file.

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
authentication:
  anonymous:
    enabled: false
```

- Once it is set to `false`, we can no longer access the Kubelet API without authentication. There are two ways to authenticate to the Kubelet API:
  - Using a client certificate(Mostly used)
  - Using a Bearer token

- We need to provide a valid client certificate to access the Kubelet API. There will be ca cert specification in the kubelet service file which will be used to verify the client certificate. We can find the ca cert specification in the Kubelet service file by looking for the `--client-ca-file` flag. Or we can also find it in the Kubelet configuration file under the `authentication` section.

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
authentication:
  x509:
    clientCAFile: /etc/kubernetes/pki/ca.crt
```

- If we are disabling anonymous access by setting Anonymous authentication to `false`, it will be only applied to port `10250`. The read-only port 10255 is completely separate it does not support authentication or authorization at all, and that’s by design (which is why it's deprecated and disabled by default in newer versions). We can disable the read-only port by setting the `--read-only-port` flag to `0` in the Kubelet Service file or we can set it in the Kubelet configuration file.

#### Authorization

By default, the flag `--authorization-mode` is set to `AlwaysAllow`, which means that all requests are allowed. This should be set to `Webhook` to enable RBAC authorization.

- We can set the `--authorization-mode` flag in the Kubelet Service file or we can set it in the Kubelet configuration file.

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
authorization
    mode: Webhook
```
Date of Commit: 10/05/2025