## Whitelist Registries in Kubernetes

- In Kubernetes, we can restrict the registries from which images can be pulled from by using the various methods available in the Kubernetes ecosystem.
- By default, Kubernetes allows images to be pulled from any registry, which can lead to security vulnerabilities if untrusted images are used.

### Using Custom Validation Webhook
- We can create a custom admission controller that validates the image registry before allowing the Pod to be created.
- The admission controller can check the image registry against a whitelist of allowed registries and reject any requests that do not match.
- We can use the `ValidatingAdmissionWebhook` to implement this functionality. We can create a webhook server that listens for admission requests and validates the image registry.
- The webhook server can be deployed as a separate Pod in the cluster and configured to receive admission requests from the Kubernetes API server or we can also deploy it as a standalone service outside the cluster.

**Example:**
```py
@app.route("/validate", methods=["POST"])
def validate():
    image_name = request.json["request"]["object"]["spec"]["containers"][0]["image"]
    status = True
    message = ""
    if "internal-registry.io" not in image_name:
        message = "You can only use images from the internal-registry.io"
        status = False
    return jsonify(
        {
            "response": {
                "allowed": status,
                "uid": request.json["request"]["uid"],
                "status": {"message": message},
            }
        }
    )
```

### Using OPA 

By using OPA with admission controller we can enforce our own custom policies in rego for allowing or denying requests to the Kubernetes API server. OPA can be used to validate the image registry before allowing the Pod to be created.
- We can create a rego policy that checks the image registry against a whitelist of allowed registries and reject any requests that do not match.
```rego
package kubernetes.admission


deny[msg] {
    input.request.kind.kind == "Pod"
    image := input.request.object.spec.containers[_].image
    not startswith(image, "internal-registry.io/")
    msg := sprintf("Image '%s' is not from a trusted registry", [image])
}
```

### Using ImagePolicyWebhook
- Kubernetes provides a built-in `ImagePolicyWebhook` admission controller that can be used to validate image registries.
- The `ImagePolicyWebhook` can be configured to call an external webhook server that validates the image registry before allowing the Pod to be created.

#### Steps to configure ImagePolicyWebhook:

- Create a webhook server that listens for admission requests and validates the image registry.
- Create a AdmissionConfiguration object that specifies the rules for the `ImagePolicyWebhook` like TTL, retry backoff, default allow and also the path to the kubeconfig file where the details of the webhook server are specified. So that webhook can connect to the custom webhook server.
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
- name: ImagePolicyWebhook
  configuration:
    imagePolicy:
      kubeConfigFile: <path-to-kubeconfig-file>
      allowTTL: 50
      denyTTL: 50
      retryBackoff: 500
      defaultAllow: true # If the webhook is not reachable, allow the request by default
```
- Create a `KubeConfig` file that specifies the URL of the webhook server and the CA certificate for the webhook server.
```yaml
clusters:
- name: name-of-remote-imagepolicy-service
  cluster:
    certificate-authority: /path/to/ca.pem
    server: https://images.example.com/policy
users:
- name: name-of-api-server
  user:
    client-certificate: /path/to/cert.pem
    client-key: /path/to/key.pem
```
- Once it is done, we can now enable the `ImagePolicyWebhook` admission controller in the Kubernetes API server by adding the following flags to the API server command line:
```bash
--admission-control-config-file=/path/to/admission-configuration.yaml
--enabled-admission-plugins=ImagePolicyWebhook
```

Date of Commit: 19/05/2025