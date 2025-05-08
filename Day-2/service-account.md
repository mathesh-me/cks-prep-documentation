## Service Accounts in Kubernetes

### Types of Accounts in Kubernetes:

**1. User Accounts:** Will be used by Humans<br>
**2. Service Account:** It is a special type of account intended to represent a **non-human user**, such as a pod or application, that needs to interact with the Kubernetes API. Unlike regular user accounts, service accounts are managed by Kubernetes itself.

### Purpose

`Service accounts` are used by:

- Pods to authenticate to the Kubernetes API
- Workloads to access cluster resources securely
- Controllers and automated tools (e.g., CI/CD) to interact with the cluster

### Command for working with Service Account:

**For Creating a Service Account**
```bash
kubectl create serviceaccount my-service-account
```

**For Listing all Service Accounts**
```bash
kubectl get sa
```

**Describe a specific service account**
```bash
kubectl describe serviceaccount my-service-account
```

**View mounted token in a pod**
```bash
kubectl exec -it pod-name -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
```

### Default Behavior

When a pod is created and no service account is specified, it is automatically assigned the **default** service account in the same namespace. The pod will use the token associated with the default service account. We can also use our custom service account by modifying the pod definition file. For deployments, if we modify the manifest file, it will automatically perform the rollouts for us. For pods, we need to delete the pod and recreate it again.

### Service Account Tokens

#### Before Kubernetes v1.22 (Legacy Token Behavior)

When you create a ServiceAccount, Kubernetes automatically creates a secret containing a static JWT token.<br>

This token is:

- Mounted into pods at: /var/run/secrets/kubernetes.io/serviceaccount/token
- Long-lived (no expiration)
- Stored as a Kubernetes Secret (type kubernetes.io/service-account-token)

#### Kubernetes v1.22

Kubernetes introduced Bound Service Account Tokens as a beta feature in v1.21 and enabled by default in v1.22.

**Key Features:**
- Short-lived tokens (default: 1 hour)
- Not stored as secrets
- Bound to:
    - A specific pod
    - A specific audience (e.g., Kubernetes API, OIDC provider)
    - Designed to support external identity providers and cloud-native security (e.g., EKS IRSA, GKE Workload Identity)

#### Kubernetes v1.24

As of Kubernetes v1.24, the automatic creation of secret-based service account tokens was removed. You must explicitly create tokens if needed.

**Key Changes:**
- No more kubernetes.io/service-account-token secrets by default
- Must use TokenRequest API or kubectl create token

#### What Actually Changed from v1.22 to v1.24?

**v1.22:**
- BoundServiceAccountToken feature became enabled by default.
- Kubernetes still auto-created token Secrets for backward compatibility.
- So even if you didn’t use TokenRequest, pods still got tokens via secrets.

> You could still rely on legacy tokens, but encouraged to start using dynamic tokens.

**v1.24:**
- Kubernetes stopped automatically creating the token Secret.
- You must explicitly request a token using kubectl create token or the TokenRequest API.
- Pods now mount a projected token (short-lived), not a static one.

> Any code/config expecting a Secret named my-service-account-token-xxxxx will break if not updated.

| Feature / Behavior                    | Kubernetes v1.22                                 | Kubernetes v1.24                          |
| ------------------------------------- | ------------------------------------------------ | ----------------------------------------- |
| **Auto-creation of Secret token**  | ✅ Yes (but deprecated behavior)                  | ❌ No — auto-creation is **removed**       |
| **BoundServiceAccountToken**       | ✅ Enabled by default                             | ✅ Still enabled by default                |
| **Token stored in Secret object**  | ✅ Yes (still created for backward compatibility) | ❌ No (no secret is created automatically) |
| **TokenRequest API available**     | ✅ Available & supported                          | ✅ Same                                    |
| **Token expiration**                | ✅ Supported via `TokenRequest`                   | ✅ Same                                    |
| **Pod token mounting (projected)** | ✅ Supported with projected volume                | ✅ Same                                    |
| **Token rotation**                 | ✅ Yes                                            | ✅ Yes                                     |
| **kubectl create token** available | ✅ Yes                                            | ✅ Yes                                     |

Date of Commit: 30/04/2025
