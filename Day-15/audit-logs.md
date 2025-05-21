## Auditing in Kubernetes

- Auditing is the process of tracking and logging the actions taken on a Kubernetes cluster. It provides a way to monitor and analyze suspicious activity in the cluster.
- Kubernetes support auditing through the kube-apiserver. The kube-apiserver can be configured to log all the requests made to the API server and the responses returned by the API server. It is disabled by default. We need to enable it explicitly.

### Different stages in Kubernets Request

1. Request Received: When a request is made—such as creating a new Nginx pod—it first reaches the kube-apiserver.
```bash
kubectl run nginx --image=nginx
```
At this point, the request enters the "Request Received" stage, and an event is logged—even if the request is invalid.

2. Request Recorded: The moment the kube-apiserver receives the request, it is logged as a "request received" event, regardless of its outcome or validity.

3. Response Started: If the request passes authentication, validation, and authorization, a "response started" event is triggered. This is especially helpful for tracking long-running operations, such as when using --watch to monitor resources in real time.

4. Response Complete: Once the request is fully processed and a response is returned to the client, a "response complete" event is generated, indicating successful completion.

5. Panic Stage: If an unexpected error occurs during processing or if the request is invalid, it enters the "panic" stage, signaling a failure or critical issue.

Each of these stage will generate lot of events. Logging everything is not a good idea. So we can filter the events based on the stage and log only the required events. We can also filter the events based on the user, resource, and other parameters. This will help us to reduce the size of the audit logs and make it easier to analyze the logs.

### Audit Policy

- The audit policy is a set of rules that define what events should be logged and how they should be logged. The audit policy is defined in a YAML file. The default audit policy file is located at `/etc/kubernetes/audit-policy.yaml`. We can create our own custom audit policy file and specify it in the kube-apiserver configuration.

- The audit policy file is divided into two sections: `rules` and `omit`. The `rules` section defines the rules for logging the events. The `omit` section defines the rules for omitting the events. The `omit` section is optional.

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: Metadata
    namespace: kube-system
    verbs:
      - create
      - update
    omitStages:
      - RequestReceived
      - ResponseStarted
    resources:
      - group: ""
        resources: ["pods"]
  - level: Request
    omitStages:
      - ResponseStarted
    resources:
      - group: ""
        resources: ["pods"]
  - level: RequestResponse
    omitStages:
      - ResponseStarted
    resources:
      - group: ""
        resources: ["pods"]
```
- The `level` field defines the level of logging. The possible values are:
  - `None`: No logging.
  - `Metadata`: Log only the metadata of the request and response.
  - `Request`: Log the request and response.
  - `RequestResponse`: Log the request and response with the body.
- The `namespace` field defines the namespace to log. This can be a specific namespace or all namespaces.
- The `verbs` field defines the verbs to log. This can be a list of verbs or a wildcard (*). The possible values are:
  - `get`: Log GET requests.
  - `list`: Log LIST requests.
  - `watch`: Log WATCH requests.
  - `create`: Log CREATE requests.
  - `update`: Log UPDATE requests.
  - `patch`: Log PATCH requests.
  - `delete`: Log DELETE requests.
- The `omitStages` field defines the stages to omit from logging. The possible values are:
    - `RequestReceived`: Omit the request received stage.
    - `ResponseStarted`: Omit the response started stage.
    - `ResponseComplete`: Omit the response complete stage.
    - `Panic`: Omit the panic stage.
- The `resources` field defines the resources to log. The possible values are:
    - `group`: The API group of the resource.
    - `resources`: The resources to log. This can be a list of resources or a wildcard (*).

### Audit Backend

- The audit backend is the location where the audit logs are stored. There are two types of audit backends:
  - `log`: The audit logs are stored in a file. This is the default backend.
  - `webhook`: The audit logs are sent to a webhook server. This can be used to send the audit logs to a central logging server.

### Example for Audit Log Backend

By specifying the below configuration in the kube-apiserver, we can enable audit logging and specify the audit policy file and the audit backend.

```yaml
--audit-policy-file=/etc/kubernetes/audit-policy.yaml
--audit-log-path=/var/log/audit.log
--audit-log-maxage=30
--audit-log-maxbackup=10
--audit-log-maxsize=100
```

- The `--audit-policy-file` flag specifies the path to the audit policy file.
- The `--audit-log-path` flag specifies the path to the audit log file.
- The `--audit-log-maxage` flag specifies the maximum number of days to keep the audit logs.
- The `--audit-log-maxbackup` flag specifies the maximum number of backup files to keep.
- The `--audit-log-maxsize` flag specifies the maximum size of the audit log file in MB. Once the file reaches this size, it will be rotated and a new file will be created.

Once the audit logging is enabled, we can view the audit logs in the specified file.

Date of Commit: 21/05/2025


