## Cilium Encryption Policies\

Just like Network Policies, Cilium also provides a way to define encryption policies for pod-to-pod communication. These policies can be used to specify which pods should have their traffic encrypted and the type of traffic that should be encrypted. 

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: allow-encrypted-traffic
spec:
  endpointSelector:
    matchLabels:
      app: myapp
  egress:
  - toEndpoints:
    - matchLabels:
        app: myapp
    toPorts:
    - ports:
      - port: "80"
        protocol: TCP
```

This policy allows encrypted traffic to and from pods with the label `app: myapp`. The traffic is encrypted using the encryption method specified in the Cilium configuration.

### Verifying Encryption

We can verify that encryption is working by using `tcpdump` or similar tools to capture network traffic between pods. The captured traffic should be encrypted and not readable in plain text.

Date of Commit: 18/05/2025