## Mutual TLS (mTLS)

Mutual TLS (mTLS) is an extension of the TLS (Transport Layer Security) protocol that provides two-way authentication and encryption between a client and a server. In a typical TLS handshake, only the server presents its certificate to the client for authentication. In mTLS, both the client and server present their certificates to each other, allowing both parties to verify each other's identity.

### Pod to Pod Communication Encryption using mTLS

- By default, Kubernetes does not encrypt traffic between pods. However, we can use mTLS to encrypt traffic between pods. We can certificates to each pod and configure them to use mTLS for communication. But when a number of pods increases, managing certificates for each pod becomes cumbersome. So, we can use a service mesh like Istio or Linkerd to manage mTLS for us.
- Service mesh tools abstract encryption responsibilities away from the application layer to network layer. They automatically inject sidecar proxies into pods, which handle mTLS encryption and decryption. This allows developers to focus on writing code without worrying about managing certificates and encryption.

### Istio mTLS

- Istio is a popular service mesh that provides mTLS for pod-to-pod communication. It uses Envoy as a sidecar proxy to handle mTLS encryption and decryption.
- Istio operates in two modes:
  - **Strict Mode**: In this mode, all traffic between pods is encrypted using mTLS. If a pod does not have a valid certificate, it cannot communicate with other pods.
  - **Permissive Mode**: In this mode, Istio allows both encrypted and unencrypted traffic. This is useful for pod that have need to communicate with other pods or services that do not support mTLS.
- **Working of Istio mTLS**:
  - When a pod wants to communicate with another pod, it sends a request to the Envoy sidecar proxy.
  - The Envoy proxy checks if the destination pod has a valid certificate.
  - If the destination pod has a valid certificate, the Envoy proxy establishes an mTLS connection and forwards the request to the destination pod.
  - If the destination pod does not have a valid certificate, the Envoy proxy rejects the request.
  - Once the mTLS connection is established, the Envoy proxy encrypts the traffic between the two pods by intercepting the traffic and encrypting it using the certificates.

Date of Commit: 17/05/2025