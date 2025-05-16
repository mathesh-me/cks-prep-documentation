## K8S Secrets

- Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing confidential information in a Secret is safer and more flexible than putting it as a plain text in a Pod definition or in a container image.
- Creation of a Secret is similar to that of a ConfigMap. But we have to store the sensitive data in base64 encoded format.
- We can use a Secret as environment variables, as a file in a volume, or as a command-line argument to a container.

### Create a Secret

- We can create a Secret using the `kubectl create secret` command.
- Use the below command to create a Secret from a literal value:   
```shell
kubectl create secret generic <secret-name> --from-literal=<key>=<value>
```

- Use the below command to create a Secret from a file:
```shell
kubectl create secret generic <secret-name> --from-file=<path-to-file>
```

- Using Definition file:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: <secret-name>
type: Opaque
data:
  <key>: <base64-encoded-value>
```

### View a Secret

- Use the below command to view a Secret:
```shell
kubectl get secrets
```

### View the details of a Secret

- Use the below command to view the details of a Secret, But it will not show the actual data:
```shell
kubectl describe secret <secret-name
```

- To view the actual data, use the below command:
```shell
kubectl get secret <secret-name> -o yaml
```

**Using a Secret as environment variables**

- It is similar to using a ConfigMap as environment variables.
- Refer ConfigMap section for more details.

### Using Secret in a Pod

- We can either use a Secret as environment variables or mount it as a volume in a Pod.
- Use the below command to create a Pod using a Secret as environment variables:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <pod-name>
spec:
  containers:
  - name: <container-name>
    image: <image-name>
    # For using a specific key in a secret as an environment variable
    env:
    - name: <env-var-name>
      valueFrom:
        secretKeyRef:
          name: <secret-name>
          key: <key>
    # For using whole secret as environment variables
    envFrom:
    - secretRef:
        name: <secret-name>
    # For using a secret as a file in a volume
    volumeMounts:
    - name: <volume-name>
      mountPath: <mount-path>
  volumes:
    - name: <volume-name>
      secret:
        secretName: <secret-name>
```

> Users and Service Accounts create Pod and Deployments can access secrets in the same namespace. So we need to use RBAC to limit access.

Date of Commit: 16/05/2025