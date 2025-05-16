## Open Policy Agent(OPA)

OPA helps us to manage our authorization policies in a centralized way. Services can query OPA to check if a request is allowed or not. OPA acts as cetralized decision maker for authorization policies.

### Without OPA

Below is a simple example of python flask application that checks if a user is allowed to access a /home path or not.

```python
@app.route('/home')
def hello_world():
    user = request.args.get("user")
    if user != "john":
        return 'Unauthorized!', 401
    return 'Welcome Home!', 200
```
In this example, the application is responsible for checking if the user is allowed to access the /home path. If the user is not allowed, the application returns a 401 Unauthorized response. This approach works well for small applications, but as the application grows, it becomes difficult to manage authorization policies in a centralized way.

### With OPA

### Deploying OPA

- Download the OPA binary and make it executable.
```bash
curl -L -o opa https://github.com/open-policy-agent/opa/releases/download/v0.11.0/opa_linux_amd64
chmod 755 opa
```
By default, OPA Server don't have any authentication or authorization. We need to enable authentication and authorization for OPA server by ourself. 
- Run OPA in server mode.
```bash
./opa run -s
```
- Tesing OPA Policy
```bash
./opa test -v
```



#### Define authorization policy

- Create a policy file called `policy.rego` with the following content.
```rego
package httpapi.authz


# HTTP API request
import input


default allow = false


allow {
    input.path == "home"
    input.user == "john"
}
```

- Loading the policy into OPA.
```bash
curl -X PUT --data-binary @policy.rego localhost:8181/v1/policies/httpapi/authz
```
- Listing the policies loaded into OPA.
```bash
curl localhost:8181/v1/policies
```

#### Integrating OPA with the application

- Now we can remove the authorization logic from the application and delegate it to OPA. Below is the updated code for the application.
```python
@app.route('/home')
def hello_world():
    user = request.args.get("user")
    input_dict = {
        "input": {
            "user": user,
            "path": "home"
        }
    }
    rsp = requests.post("http://127.0.0.1:8181/v1/data/httpapi/authz", json=input_dict)
    if not rsp.json()["result"]["allow"]:
        return 'Unauthorized!', 401
    return 'Welcome Home!', 200
```

In the above example, The application will send a request to the OPA query endpoint with the input data. OPA will evaluate the policy and return a response indicating whether the request is allowed or not. The application will then decide whether to allow or deny the request based on the response from OPA.

Date of Commit: 15/05/2025