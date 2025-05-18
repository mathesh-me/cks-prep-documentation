## Cilium

- Cilium is an open-source tool that provide secure network connectivity between container workloads. It is built on top of the Linux kernel's eBPF (extended Berkeley Packet Filter) technology, which allows for high-performance networking and security features with minimal overhead. 
- Cilium can enbale encrption between pod without any additional changes to the application code. 
- Multiple encryption methods are supported by Cilium, including IPsec and WireGuard.
- We can also customize our encryption policies based on the specific needs of our applications.

### Cilium Setup

- Cilium can be installed using Helm or by applying the Cilium YAML manifest directly to the cluster.
- The following command can be used to install Cilium using Helm:
```bash
helm repo add cilium https://helm.cilium.io/
helm repo update
helm install cilium cilium/cilium --version 1.12.0 \
  --namespace kube-system \
  --set global.hubble.enabled=true \
  --set global.hubble.relay.enabled=true \
  --set global.hubble.ui.enabled=true
```
- Once installed, enable encryption by applying the following command:
```bash
cilium encrypt enable
```
- It also provide key management service, but we can also use external key management services like HashiCorp Vault or AWS KMS.
- Once it is done, We can create our encryption policies based on namespace, or labels to specify where encryption should be applied and when it will be applied.
- Apart from encryption, Cilium also provides other features such as load balancing, monitoring, and identity management.

Date of Commit: 18/05/2025