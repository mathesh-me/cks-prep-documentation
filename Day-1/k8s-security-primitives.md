## Kubernetes Security Primitives

### Security Measures in Kubernetes

#### Securing Hosts

- Infrastructure that helps run Kubernetes clusters, such as virtual machines or bare-metal servers, should be secured. This includes hardening the operating system, applying security patches, and ensuring that only necessary services are running.
- Disabling root access and password-based authentication and use SSH key-based authentication.

> If the host is compromised, the entire Kubernetes cluster can be at risk.

#### API Server Security

- The API server is the central management component of Kubernetes. It exposes the Kubernetes API and serves as the entry point for all administrative tasks.
- Most of the operations in Kubernetes are done through the API server, so securing it is crucial.
- Users interact with Cluster through the API server, and it is responsible for validating and processing API requests. So, there should be proper authentication and authorization in place for `who can acccess the Cluster` and `what they can perform`.

#### Authentication and Authorization

- AUthentication is the process of verifying the identity of a user or system trying to access the Kubernetes API server. Kubernetes supports multiple authentication methods, including certificates, tokens, password and username and external authentication providers (like LDAP or OIDC).
- Service accounts are used for machine-to-machine communication within the cluster. 
- Authorization is the process of determining whether a user or system has permission to perform a specific action. Kubernetes supports several authorization modes, including RBAC (Role-Based Access Control), ABAC (Attribute-Based Access Control), Node Authorization and Webhook mode.

#### Inter Cluster Communication Security

- All communication between the components of a Kubernetes cluster will be protected by TLS encryption. 

#### Network Policies

- By default, all pods in a Kubernetes cluster can communicate with each other. Network policies are used to control the communication between pods and services.
- Help us to Secure communication between applications in Kubernetes Cluster

Commit Date: 29/04/2025