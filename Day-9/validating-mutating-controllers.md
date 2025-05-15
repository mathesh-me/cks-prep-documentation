## Validating and Mutating Admission Controllers

Types of Admission Controllers:
- **Validating Admission Controllers**: These controllers validate the requests and can reject them if they do not meet certain criteria. They do not modify the request.
    - Example: `NamespaceExists`.
- **Mutating Admission Controllers**: These controllers can modify the request before it is persisted in etcd. They can add, remove, or modify fields in the request.
    - Example: `DefaultStorageClass`.
- In some cases, a single admission controller can be both validating and mutating. For example, `PodSecurityPolicy` can validate the request and also modify the security context of the Pod.

### Using Webhooks to Implementing Custom Logic

- We can implement custom admission controllers using two types of webhooks:
    - **Validating Admission Webhook**
    - **Mutating Admission Webhook**

Now we can direct our requests to custom servers. We will implement our cutsom admission logic in the server. After the request is processed by Built-in admission controllers, it will be sent to our custom server. The server will then process the request and send a response back to the API server. The API server will then decide whether to accept or reject the request based on the response from the webhook. We need to configure our custom server to accept the request and send a response back to the API server.

**Configuring Webhook:**

After the cutsom server is implemented, we need to configure our cluster to use the webhook. We can do this by creating a `ValidatingWebhookConfiguration` or `MutatingWebhookConfiguration` resource in the cluster. This resource will contain the URL of the custom server and the rules for when to send requests to the server.
**Example:**
```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: "pod-policy.example.com"
webhooks:
  - name: "pod-policy.example.com"
    clientConfig:
      # If not deployed as Pod in the cluster, we can use the URL of the server
      url: "https://webhook.example.com/admission"
      # If deployed as Pod in the cluster, we can use the service name and namespace
      service:
        namespace: "webhook-namespace"
        name: "webhook-service"
      caBundle: "CiOtLS0tQk......tLS0K" # We need to generate certificate to ensure secure communication between API server and webhook server
    rules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
        scope: "Namespaced"
```

Date of Commit: 15/05/2025