## Auditing in Kubernetes

Auditing in Kubernetes helps track and log the actions taken on the cluster, providing a way to monitor and review changes for security and compliance purposes. It is essential for understanding who did what in the cluster and when.

### Audit Policies

- **None**  
  No events are logged.

- **Metadata**  
  Only request metadata is logged, such as user, timestamp, resource, and verb. Request and response bodies are not included.

- **Request**  
  Logs both the event metadata and the request body. The response body is not logged.

- **RequestResponse**  
  Logs everything: event metadata, the request body, and the response body.

**Example Audit Policy:**
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: Metadata
    resources:
      - group: ""
        resources: ["pods"]
    verbs: ["get", "list"]
  - level: Request
    resources:
      - group: ""
        resources: ["pods"]
    verbs: ["create", "update", "delete"]
```
- **Level**: Specifies the level of detail to log.
- **Resources**: Specifies the resources to which the rule applies.
- **Verbs**: Specifies the actions to log (e.g., get, list, create, update, delete).
- **Group**: Specifies the API group of the resource.

Date of Commit: 12/05/2025