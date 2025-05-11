## Network Policy

- Network policy is a type of object in Kubernetes that allows us to control network tarffic between pods. It is a way to define rules for how pods can communicate with each other and with other network endpoints.
- Example Use Case: Normally Every pod can communicate with every other pod in the cluster. But if we want to restrict the communication between the pods. Then we can use the network policy to define the rules for the communication between the pods.

### Creating Network Policy:

- Definition file for Network Policy:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-nginx
spec:
  podSelector:   # Select the pods to which the policy should be applied
    matchLabels:
    app: nginx
    
  policyTypes:   # Types of the policy. It can be Ingress, Egress or both
  - Ingress
  
  ingress:       # Rules for the incoming traffic
  - from:
    - podSelector:   # Select the pods from which the traffic is allowed
        matchLabels:
        app: myapp
      namespaceSelector:   # Select the namespace from which the traffic is allowed
        matchLabels:
        project: myproject
    
    - ipBlock:   # Allow the traffic from the specific IP block
        cidr: 193.10.0.0/32
    ports:
    - protocol: TCP
    port: 80
```
- In the above example, we are allowing the traffic to the pods with the label `app: nginx` from the pods with the label `app: myapp` in the namespace with the label `project: myproject` or from the IP block.
- We can create the network policy using the following command:

```bash
kubectl create -f network-policy.yaml
```

Date of Commit: 11/05/2025