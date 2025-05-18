## Additional Considerations in Multi Tenant Kubernetes Clusters

When deploying applications in a multi-tenant Kubernetes cluster, there are several additional considerations to keep in mind:

- **API Priority and Fairness**: This feature allows you to prioritize requests from different users or teams, ensuring that critical workloads receive the necessary resources even during high load periods.
- **Pod Priority and Preemption**: This feature allows you to assign priority levels to pods, ensuring that higher-priority pods can preempt lower-priority ones if resources are

### API Priority and Fairness

Since Kubernetes manages all operations through single API endpoint, it is essential to ensure that required API requests are grtting higher priority. For example, if a team is deploying a critical application, it should be able to get the required resources even if other teams are making requests at the same time. API Priority and Fairness allows us to assign different priority levels to API requests, ensuring that critical requests are processed first. This is particularly important in multi-tenant environments where multiple teams may be competing for the same resources.

- Create a `PriorityLevelConfiguration` object that defines the priority levels and their corresponding configurations.

```yaml
apiVersion: flowcontrol.apiserver.k8s.io/v1beta3
kind: PriorityLevelConfiguration
metadata:
  name: high-priority
spec:
  type: Limited
  limited:
    assuredConcurrencyShares: 10  # High priority gets more concurrency
    limitResponse:
      type: Queue
```
- Create a `FlowSchema` object that defines the rules for assigning priority levels to requests.

```yaml
apiVersion: flowcontrol.apiserver.k8s.io/v1beta3
kind: FlowSchema
metadata:
  name: high-priority-namespace-a
spec:
  priorityLevelConfiguration:
    name: high-priority  # Link to high priority
    matchingPrecedence: 1000
  rules:
    - subjects:
        - kind: ServiceAccount
          name: "system-account"  # Alternatively, match on user or service account
          namespace: "namespace-a"  # Target Namespace A
      resourceRules:
        - verbs: ["*"]
          apiGroups: ["*"]
          resources: ["*"]
```

This ensures that requests from Namespace A are given higher priority than those from other namespaces. You can create similar `FlowSchema` objects for other namespaces with different priority levels.

### Pod Priority and Preemption

It is used to manage node allocation(CPU and Memory) in a multi-tenant environment. By assigning priority levels to pods, It helps ensure that critical pods can get the resources they need, even if it means preempting lower-priority pods.<br>

This can be done by creating a `PriorityClass` object that defines the priority level and then assigning it to the pods that need it. For example, you can create a `PriorityClass` object for high-priority pods:

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class should be used for high-priority pods."
```

Then, you can assign this `PriorityClass` to the pods that need it:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: high-priority-pod
spec:
  priorityClassName: high-priority
  containers:
    - name: my-container
      image: my-image
```

Date of Commit: 18/05/2025
