## Cilium Architecture

Cilium uses eBPF (extended Berkeley Packet Filter) technology that runs directly in the Linux kernel. This allows Cilium to provide high-performance networking and security features with minimal overhead. It acts as a CNI plugin, handling how pods communicate, while also enforcing security policies to make sure only allowed traffic gets through.
### Key Features:

- **Network Policies**: Cilium allows you to define network policies using labels and selectors, enabling fine-grained control over pod-to-pod communication.
- **Services and Load Balancing**: Routing and Distribute loads among pods equally.
- **Bandwidth Management**: Cilium can manage network bandwidth for pods, allowing you to set limits on the network bandwidth that a pod can use.
- **Flow and Policy Logging**: Monitors network traffic and gain insights into policy enforcement across the cluster.
- **Security and Operational Metrics**: Visibility into Cluster security posture and overall performance.

Date of Commit: 18/05/2025