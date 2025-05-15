## Pod Security Policies

Pod Security Policies are used to validate Pod Creation and Update requests. They are used to enforce security policies for Pods. When PSP admission controller is enabled, it will validate the requests against the defined Pod Security Policies. If the request does not meet the criteria defined in the PSP, it will be rejected. To enable the PSP admission controller, we need to add `PodSecurityPolicy` to the `--enable-admission-plugins` flag in the API server configuration. 

### Problem with Directly enabling PSP

- If we directly enable the PSP admission controller, it will reject all requests for Pods if the user creating the Pod or Service account associated with the Pod does not have the required permissions to access Pod Security Policies.
- To avoid this, we need to create a Role and RoleBinding for the user or service that is creating the Pod. This Role and RoleBinding should grant the user or service the required permissions to access Pod Security Policies. Once this is done, we can enable the PSP admission controller and it will work as expected.

### Defining Pod Security Policies

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: example-psp
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: MustRunAsNonRoot
  requiredDropCapabilities:
    - CAP_SYS_BOOT
  defaultAddCapabilities:
    - CAP_SYS_TIME
```

This Policy will reject any Pod that is trying to run as root user. It will also reject any Pod that is trying to use the `CAP_SYS_BOOT` capability. It will add the `CAP_SYS_TIME` capability to the Pod by default. So this admission controller will act like both Validating and Mutating admission controller. It will validate the request and also modify the security context of the Pod.

Date of Commit: 15/05/2025