## Ingress

### What is Ingress?
- Ingress is a Kubernetes API object that manages external access to services in a cluster, usually over HTTP or HTTPS.
- Instead of exposing each service separately using a LoadBalancer or NodePort, you can define a single Ingress to route traffic based on things like:
    - Hostnames (e.g. api.example.com)
    - Paths (e.g. /login, /products)
    - HTTP methods or headers (with advanced controllers)

**Example:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

This routes traffic from myapp.com/api to the api-service on port 80.

### What is an Ingress Controller?

- In order for an Ingress to work in your cluster, there must be an ingress controller running. You need to select at least one ingress controller and make sure it is set up in your cluster.
- The Ingress Controller is the actual software (pod) that reads the Ingress rules and configures a load balancer or reverse proxy (like NGINX, Traefik, or HAProxy) to implement them. Apart from traffic routing, it can also handle SSL termination, authentication, and other features.
- Ingress controllers will listen for changes to Ingress resources and update their configuration accordingly. 
- Kubernetes does not include an ingress controller by default, so you must install one. We can deploy ingress controllers as pods in the cluster.
- Example of popular Ingress Controllers:
    - NGINX Ingress Controller – most commonly used
    - Traefik
    - HAProxy
    - Istio Gateway (acts like an advanced ingress)

Date of Commit: 11/05/2025