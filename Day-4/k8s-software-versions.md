## Kubernetes Software Versions

### Version Fortmat
- If we use `kubectl get nodes` command, we can see the Kubernetes version of the nodes.
- Kubernetes version format: `major.minor.patch`. Eg., `1.20.4`.
- The `major` version for breaking changes.
- The `minor` version for new features.
- The `patch` version for bug fixes.
- There is also two more versions: `alpha` and `beta`.
- `alpha` and `beta` versions are for testing new features.
- The `alpha` version is for testing new features that are not yet stable.
- The `beta` version is for testing new features that are stable but not yet production ready.
- Once the features in the `beta` version are stable, they will be released in the next `minor` version.

### Other Notes
- Only 3 versions are supported at a time. The oldest version will be removed when a new version is released.
- All the Kubernetes components like **kube-apiserver, kube-controller-manager, kube-scheduler, kubelet, kube-proxy** should be of the same version as the Kubernetes version of the nodes.
- **etcd** and **CoreDNS** Versions might be different from the Kubernetes version as they are separate projects. Supported versions of these twp components for a particular Kubernetes version can be found in the Kubernetes release notes of release page.

Date of Commit: 10/05/2025