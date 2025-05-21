## Detect Threats using Falco

- Verify falco is running
```bash
systemctl status falco
```
- Once falco is running, we can check the logs to see if any malicious activity is detected.
```bash
journalctl -f -u falco
```
`-f` flag is used to follow the logs in real-time. We can see the logs of falco in real-time.

### Generate Alerts

- Create a pod and run a command that will trigger an alert in falco.
```bash
kubectl run -i --tty --rm debug --image=ubuntu --restart=Never -- bash
```

### Falco Rules

- Falco uses rules to detect any malicious activity. The rules are defined in the falco configuration file. The default configuration file is located at `/etc/falco/falco_rules.yaml`.
- We can create our own custom rules to detect any malicious activity.

**Example: Detect Shell inside a container**
- We can create a rule to detect if a shell is opened inside a container. This can be used to detect if an attacker is trying to gain access to the container.
```yaml
- rule: Detect Shell inside a container
  desc: Alert if a shell such as bash is open inside a container
  condition: container and proc.name in (linux_shells)
  output: Bash Opened (user=%user.name container=%container.id)
  priority: WARNING


- list: linux_shells
  items: [bash, zsh, ksh, sh, csh]


- macro: container
  condition: container.id != host
```

**Overview of the rule:**
- `rule`: The name of the rule.
- `desc`: A description of the rule.
- `condition`: The condition that needs to be met for the rule to trigger. In this case, we are checking if a shell is opened inside a container.
- `output`: The output that will be displayed when the rule is triggered. In this case, we are displaying the user and container id. There are some special filters available in falco that can be used to filter the output. For example, `user.name` will give the name of the user who opened the shell and `container.id` will give the id of the container where the shell is opened.
- `priority`: The priority of the rule. In this case, we are setting the priority to WARNING. The lowest is `DEBUG`, 
- `list`: A list of items that can be used in the condition. In this case, we are defining a list of linux shells.
- `macro`: A macro that can be used to simplify the condition. In this case, we are defining a macro to check if the process is running inside a container.

### Available Priority Levels

| Priority    | Description                                  |
| ----------- | -------------------------------------------- |
| `EMERGENCY` | System is unusable; immediate action needed. |
| `ALERT`     | Immediate attention required.                |
| `CRITICAL`  | Critical conditions.                         |
| `ERROR`     | Error conditions.                            |
| `WARNING`   | Warning conditions.                          |
| `NOTICE`    | Normal but significant condition.            |
| `INFO`      | Informational message.                       |
| `DEBUG`     | Debug-level message; lowest importance.      |

Date of Commit: 20/05/2025