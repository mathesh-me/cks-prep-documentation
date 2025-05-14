## AppArmor in K8S

### Pre-requisites

To use AppArmor Profiles in Kubernetes, we need to ensure that the following pre-requisites are met:

1. The Host System have AppArmor installed and running.
2. AppArmor profiles are loaded and available on the host system.
3. The Container runtime supports AppArmor (e.g., Docker, containerd).

### Use AppArmor in Kubernetes

- Create a AppArmor profile:
```
profile apparmor-deny-write flags=(attach_disconnected) {
    file,
    # Deny all file writes.
    deny /** w,
}
```
- Load the AppArmor profile:
```bash
sudo apparmor_parser /path/to/apparmor-deny-write
```
- Create a Pod with AppArmor profile:

**Legacy Configuration**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
  annotations:
    container.apparmor.security.beta.kubernetes.io/ubuntu-sleeper: localhost/apparmor-deny-write
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu
      command: [ "sh", "-c", "echo 'Sleeping for an hour!' && sleep 1h" ]
```

**New Configuration**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: apparmor-deny-write
  containers:
    - name: ubuntu-sleeper
      image: ubuntu
      command: [ "sh", "-c", "echo 'Sleeping for an hour!' && sleep 1h" ]
```

Date of Commit: 14/05/2025