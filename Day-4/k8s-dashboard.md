## Kubernetes Dashboard

- The Kubernetes Dashboard is a web-based user interface that allows us to manage and monitor our Kubernetes cluster. It provides a graphical representation of the cluster resources and allows us to perform various operations like creating, updating, and deleting resources.
- The dashboard is not installed by default in Kubernetes, and we need to install it manually. 
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

### Accessing the Dashboard

- By default, the dashboard is not accessible from outside the cluster. The Service that is created for the dashboard is of type `ClusterIP`, which means it can only be accessed from within the cluster. So to expose the dashboard to the outside world, we have three options:
  - Change the Service type to `NodePort`: Secure in Corporate Environment for accessing the dashboard as a team.
  - Change the Service type to `LoadBalancer`: Not Recommended.
  - Using `Authentication Proxy`: Good but Complex to set up.
- Once the dashboard is exposed via one of the above methods, we can access the dashboard by using the appropriate URL based on the method used to expose it. Now we can access the dashboard using either token or kubeconfig file.

Date of Commit: 10/05/2025
