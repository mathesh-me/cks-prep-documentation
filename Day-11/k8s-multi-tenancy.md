## Multi Tenancy in K8S

### Multi Tenancy

Multi-tenancy in Kubernetes refers to Sharing a single Kubernetes cluster among multiple apps, teams, or organizations. It allows different teams or applications to run on the same cluster while maintaining isolation and security between them. Multi-tenancy is important for resource optimization, cost savings, and efficient management of Kubernetes clusters.

### Types of Multi Tenancy

- **Multi Team Tenancy**: Cluster is shared among multiple teams or project within an organization. Each team has its own set of resources and permissions. RBAC and Resopurce Quotas are essential to manage access and resource allocation between internal teams. In this type, Internal teams will have access to the cluster through kubectl or GitOps Controllers. Will be governed by the organization’s policies and security measures.
- **Multi Organization Tenancy**: Cluster is shared among multiple organizations. This approach is used by SaaS providers to provide Kubernetes clusters to their customers. This type needs more strict isolation and security measures compared to multi-team tenancy. Since no direct access to the cluster is provided, ensuring stringent security protocols (such as compliance with GDPR, HIPAA, or other regulatory standards) is essential.
### Isolation in Multi Tenancy

- Without proper isolation and security measures, different teams or applications running on the same cluster can interfere with each other, leading to unauthorized data access, or over usage of resources by one tenant can lead to resource consumption, and security vulnerabilities.
- Kubernetes provides several mechanisms to achieve isolation in a multi-tenant environment, including:
  - **Namespaces**: Namespaces are virtual clusters within a Kubernetes cluster. They provide a way to divide cluster resources between multiple users or teams. Each namespace has its own set of resources, such as pods, services, and deployments.
  - **Network Policies**: Network policies are used to control the communication between pods in different namespaces. They allow you to define rules for which pods can communicate with each other, providing network isolation.
  - **Resource Quotas**: Resource quotas are used to limit the amount of resources (CPU, memory, etc.) that can be consumed by a namespace. This prevents one team or application from consuming all the resources in the cluster.
  - **RBAC (Role-Based Access Control)**: RBAC is used to control access to Kubernetes resources based on user roles. It allows you to define who can access which resources in a namespace.

Date of Commit: 17/05/2025