## Pod Security Admission and Pod Security Standards

- PSP was deprecated in Kubernetes 1.21 and removed in Kubernetes 1.25. The Pod Security Admission (PSA) controller is a replacement for PSP. It provides a simpler way to enforce security policies for Pods using Pod Security Standards (PSS). PSS are a set of predefined security profiles that can be applied to Pods to enforce security best practices. PSA is a built-in admission controller that is enabled by default.
- PSA uses labels to enforce security standards on Pods. The labels are applied to the namespace and the Pods in that namespace are required to meet the security standards defined by the labels. The three levels of Pod Security Standards are:
  - `privileged`: This level allows all capabilities and does not enforce any security policies.
  - `baseline`: This level enforces a set of security policies that are considered best practices for most workloads.
  - `restricted`: This level enforces a strict set of security policies that are considered best practices for sensitive workloads.
- Modes will determine how the Pod Security Standards are applied to the Pods in the namespace. The three modes are:
  - `enforce`: This mode will reject any Pods that do not meet the security standards defined by the labels.
  - `audit`: This mode will log any Pods that do not meet the security standards defined by the labels.
  - `warn`: This mode will log any Pods that do not meet the security standards defined by the labels and will also send a warning to the user.
- The labels are applied to the namespace using the `pod-security.kubernetes.io/<level>` label. The level can be `privileged`, `baseline`, or `restricted`. The mode is applied using the `pod-security.kubernetes.io/<mode>` label. The mode can be `enforce`, `audit`, or `warn`.
- The labels can be applied to the namespace using the `kubectl label` command. The command will look like this:
```bash
kubectl label namespace <namespace> pod-security.kubernetes.io/<level>=<mode>
```

### Exceptions in Pod Security Admission

- Sometimes, we may need to exempt certain users, namespaces, or runtime classes from the Pod Security Admission (PSA) enforcement. This can be done using the `exemptions` field in the PSA configuration.
- This is configured on the API server (cluster admin level), allowing certain users, groups, or service accounts to bypass PSA enforcement.

```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: PodSecurity
    configuration:
      apiVersion: pod-security.admission.config.k8s.io/v1
      kind: PodSecurityConfiguration
      exemptions:
        usernames:
          - admin-user
        runtimeClasses:
          - vm
        namespaces:
          - kube-system
        serviceAccounts:
          - my-namespace:privileged-sa
```

This requires changes to the kube-apiserver startup configuration, typically using `--admission-control-config-file`. **Example:** --admission-control-config-file=/etc/kubernetes/admission-config.yaml

Date of Commit: 15/05/2025