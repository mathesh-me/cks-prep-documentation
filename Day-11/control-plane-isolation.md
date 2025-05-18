## Control Plane Isolation

Control Plane isolation achieved through Kubernetes namespaces, RBAC (Role-Based Access Control), and Resource Quotas. These mechanisms ensure that different teams or applications can run on the same cluster while maintaining isolation and security between them.
- **Namespaces**: Namespaces help to logically separate cluster resources between different teams or applications. Each namespace can have its own set of resources, such as pods, services, and deployments. Resource names are unique within a namespace but can be duplicated across different namespaces. RBAC, Network Policies and many other k8s policies are namespace scoped.
- **RBAC (Role-Based Access Control)**: RBAC is used to control access to Kubernetes resources based on user roles. It determines which users and service accounts can access which resources in a namespace. 
- **Resource Quotas**: Resource quotas are used to limit the amount of resources (CPU, memory, etc.) that can be consumed by a namespace. This prevents one team or application from consuming all the resources in the cluster. Resource quotas are defined at the namespace level and can be used to enforce limits on resource usage.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-quota
  namespace: my-namespace
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "4Gi"
    limits.cpu: "4"
    limits.memory: "8Gi"
```

Date of Commit: 17/05/2025