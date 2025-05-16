## Open Policy Agent(OPA) Gatekeeper

The OPA Policy Agent Gatekeeper approach uses OPA Contraint framework along with admission controllers to enforce policies in Kubernetes clusters. 

### Installing OPA Gatekeeper

- Install OPA Gatekeeper using the following command:
```bash
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/v3.14.0/deploy/gatekeeper
```
- Verify the installation:
```bash
kubectl get pods -n gatekeeper-system
```

### OPA Contsraint Framework

OPA Constraint framework allows us to define and enforce policies in Kubernetes clusters. It provides a way to define constraints and templates that can be used to enforce policies on Kubernetes resources.
- **Constraint**: A constraint is a policy that defines a set of rules that must be followed by Kubernetes resources. It is defined using the `ConstraintTemplate` resource.
- **Template**: A template is a reusable policy that can be used to define constraints. It is defined using the `ConstraintTemplate` resource.

**OPA Constraint Framework**:

What requirements do we have? --> Where do we want to enforce the requirements? --> How do we want to enforce the requirements?<br>

- **Requirement**: All resources must have a specific label in specific namespace.
- **Where to enforce**: Since OPA Gatekeeper uses admission controllers, we can enforce the requirement at the admission controller level.
- **How**: Get the labels of the resource and check if the specific label is present in the resource. If not, reject the request.

- **Example**: Below is an example of a policy that checks if the resource has a specific label in a specific namespace. So it is our requirement,
```rego
package systemrequiredlabels


import data.lib.helpers


violation["msg": msg, "details": {"missing_labels": missing}} {
    provided := {label | input.request.object.metadata.labels[label]}
    required := {label | label == ["billing"]}
    missing = required - provided
    count(missing) > 0
    msg = sprintf("you must provide labels: %v", [missing])
}
```

In the above example, we are checking if the resource has the label `billing`. If not, we are rejecting the request with a message. The `violation` function is used to define the violation message and details. But the label and namespace are hardcoded in the policy. We want to make it dynamic. We want to enforce the polciy for all namespaces and labels. We can do this by using ConstraintTemplate and Constraint resources.

### ConstraintTemplate

- Below is an example of a ConstraintTemplate that checks if the resource has a specific label in a specific namespace. The label and namespace are passed as parameters to the template.

```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: systemrequiredlabels
spec:
  crd:
    spec:
      names:
        kind: SystemRequiredLabel
      validation:
        # Schema for the 'parameters' field goes here
        openAPIV3Schema:
          type: object
          properties:
            labels:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh # Target is the admission controller(Where we want to enforce the policy)
      # How we are enforcing the policy 
      rego: |
        package systemrequiredlabels


        import data.lib.helpers


        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.request.object.metadata.labels[label]}
          # Use the parameter passed in the constraint instead of hardcoding
          required := {label | label == input.parameters.labels[_]}
          missing = required - provided
          count(missing) > 0
          msg = sprintf("you must provide labels: %v", [missing])
        }
```

- In the above example, we are using the `input.parameters.labels` to get the labels from the constraint. The `input.parameters.labels` is passed as a parameter to the template. The `ConstraintTemplate` resource defines the schema for the parameters and the target for the policy.

### Constraint

Constraint is a resource that uses the `ConstraintTemplate` to enforce the policy. It defines the parameters for the template and the scope of the policy.
- Below is an example of a Constraint that uses the `SystemRequiredLabel` template to enforce the policy.

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: SystemRequiredLabel
metadata:
  name: require-billing-label
spec:
  match:
    namespaces: ["expensive"]
  parameters:
    labels: ["billing"]
```
- In the above example, we are using the `SystemRequiredLabel` template to enforce the policy. The `match` field is used to specify the namespaces where the policy should be enforced. The `parameters` field is used to pass the labels to the template. The policy will be enforced in the `expensive` namespace and will check if the resource has the label `billing`.

Date of Commit: 15/05/2025