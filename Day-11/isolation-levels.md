## Levels of Isolation in Kubernetes

Kubernetes provides isokation at both Control Plane and Data Plane. The isolation levels are:

- **Control Plane**: Namespaces, RBAC, Resource Quotas
- **Data Plane**: Node, Network Policies, Storage Isolation.

### Namespace Isolation

- Namespaces provide a logical separation of resources within a Kubernetes cluster. Each namespace can have its own set of resources, such as pods, services, and deployments.

### RBAC (Role-Based Access Control)

- RBAC is used to control access to Kubernetes resources based on user roles. It allows which users and service accounts can access which resources in a namespace. This is important for multi-tenancy, as it ensures that different teams or applications cannot access each other's resources.

### Resource Quotas

- Resource quotas are used to limit the amount of resources (CPU, memory, etc.) that can be consumed by a namespace. This prevents one team or application from consuming all the resources in the cluster.

### Node Isolation

- Node isolation is achieved by using taints and tolerations. Taints are applied to nodes to repel pods that do not have the corresponding toleration. This ensures that only specific pods can run on certain nodes, providing isolation between different workloads. We can dedicate nodes for specific teams or applications by applying taints to those nodes and adding tolerations to the pods that should run on those nodes.

### Network Policies

- Network policies are used to control the communication between pods in different namespaces. They allows us to define rules for which pods can communicate with each other, providing network isolation. By default, all pods can communicate with each other, but network policies can be used to restrict this communication.

### Soft vs Hard Isolation

- In soft isolation, the isolation is not strict, underlying resources are shared, and there is a possibility of resource contention. But can use resource quota and network policies to limit the resource usage and network access.
- In hard isolation, the isolation is strict, underlying resources are not shared. Each tenant has its own dedicated resources.

Date of Commit: 17/05/2025