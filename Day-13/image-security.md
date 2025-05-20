## Image Security

- Consider this simple Pod spec:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: nginx
```

- Here the image name is `nginx`, Since we didn't specify Registry, Username or Account and tag, it will use the default values:
  - Registry: `docker.io`
  - Username: `library`
  - Tag: `latest`

### Authenticating with private registries

- In order to pull images from a private registry, we need to provide authentication credentials.
```bash
docker login <registry>
```
- This command will prompt for a username and password, and store the credentials in `~/.docker/config.json`.
- Once authenticated, we can pull images from the private registry using the same `docker pull` command.

### Private Registry Authentication in Kubernetes

- Kubernetes uses a `Secret` to store the authentication credentials for a private registry.
- The `Secret` can be created using the following command:
```bash
kubectl create secret docker-registry <secret-name> \
  --docker-server=<registry> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>
```
Here the type of the secret is `docker-registry`, which is a special type of secret used for storing Docker registry credentials.
- Once the secret is created, we can reference it in our Pod spec using the `imagePullSecrets` field:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  imagePullSecrets:
    - name: <secret-name>
  containers:
    - name: mycontainer
      image: <registry>/<username>/<image>:<tag>
```

Date of Commit: 19/05/2025