## DNS in Multi-Tenant Environments

- Kubernetes implements a DNS system called CoreDNS by default, The deafult configuration of CoreDNS is any pod from any namespace can resolve the DNS name of any other pod or services in any namespace. This is not ideal for multi-tenant environments.
- We can restricts the DNS resolution only to same namespace by using the `kube-dns` service. This can be done by editing a ConfigMap in the `kube-system` namespace that contains the CoreDNS configuration. The ConfigMap is named `coredns` and can be found in the `kube-system` namespace.

```bash
kubectl create configmap coredns -n kube-system
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
data:
  Corefile: |
    .:53 {
        errors
        health {
            lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
            pods verified
            fallthrough in-namespace # Add this line to restrict DNS resolution to the same namespace
        }
        prometheus :9153
        forward . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }
```

Date of Commit: 18/05/2025