## Kubectl Proxy and Port Forward

### Kubectl Proxy

- `kubectl proxy` is a command that creates a local proxy server to access the Kubernetes API server without specifying authentication credentials manually. It will use credentials from the current context in our kubeconfig file to authenticate with the API server.
- We can also use `kubectl proxy` to access services in the cluster. Services that are exposed using `ClusterIP` can be accessed using the proxy server.
```bash
curl http://localhost:8001/api/v1/namespaces/ns_name/services/svc_name/proxy/
```

### Port Forward

- `kubectl port-forward` is a command that allows us to map our local port to a port on a service, pod, or other resource in your cluster. It will be helpfu for debugging and testing purposes.
``bash
kubectl port-forward service/nginx 28080:80
```
- This command will forward port 80 on the nginx service to port 28080 on our local machine. We can then access the service using `http://localhost:28080`.

Date of Commit: 10/05/2025