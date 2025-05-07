## 4Cs of Cloud Native Security

### 1. Cloud Security

The Infrastructure we are using to host our applcations or Kubernetes clusters. The infrastructure can be on-premise or in the cloud(Private or Public).<br>

**Security Tip**: Use network firewalls, Implement proper Authentication and Authorization, and use encryption for data at rest and in transit.

### 2. Cluster Security

If we are hosting our applications on Kubernetes, we need to secure the Kubernetes cluster.<br>

**Security Tips**:

- Protect Docker Daemon Socket
- Secure Kubernetes API Server by using implementing Authentication and Authorization.
- Network Policies and Ingress Rules for Secure Communication.
- Restrict dashboard access with proper authentication.

### 3. Container Security

The containers we are using to host our applications.<br>

**Security Tips**:

- Use trusted base images and scan for vulnerabilities. Enforce policies to only use trusted images.
- Use read-only file systems and limit the capabilities of the containers.
- Use secrets management tools to manage sensitive data.
- Disable containers to run in privileged mode.

### 4. Code Security

The code we are using to build our applications.<br>

**Security Tips**:

- Avoid hardcoding secrets in the code.
- Avoid Using sensitive data in the Environment Variables.
- Use secrets management tools to manage sensitive data.
- Use TLS encryption for data in transit and at rest.

Commit Date: 29/04/2025