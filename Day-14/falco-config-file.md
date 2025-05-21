## Falco Configuration File

- The falco configuration file is located at `/etc/falco/falco.yaml`. During the startup of falco, it will read the configuration file and load the rules defined in the configuration file.
- The configuration file is in YAML format and can be edited to customize the falco settings.
- Configuration file contains the following sections:
  - `rules_file`: The path to the rules file. This is where the rules are defined. By default, it is set to `/etc/falco/falco_rules.yaml`.
  - `json_output`: If set to true, falco will output the alerts in JSON format. This can be used to integrate falco with other tools like ELK stack.
  - `log_level`: The log level for falco. This can be set to `info`, `warning`, `error`, or `debug`. By default, it is set to `info`. And also where the logs will be sent. By default, it is set to `stdout` and `syslog`.
  - `Output Channels`: The channels where falco will send the alerts. By default, it is set to `stdout` and `syslog`. We can configure falco to send alerts to other channels like Slack, PagerDuty, and others.

### Rules File

- Falco loads all the rules from the rules files specified under the `rules_file` section in the configuration file. It is an array of rules files. All the default rules are defined in the `/etc/falco/falco_rules.yaml` file. We can create our own custom rules and add them to the rules file.
- The order of the rules in the rules file matters. If same rule appears in multiple rules files, the last rule will take precedence. So it is recommended to keep the custom rules in a separate rules file and include them in the configuration file.

### JSON Output

- By default, falco will output the alerts text format. We can configure falco to output the alerts in JSON format by setting the `json_output` to true in the configuration file. This can be used to integrate falco with other tools like ELK stack.

### Logs Configuration

- This settings are used to specify where falco will send the logs. By default, falco will send the logs to `stdout` and `syslog`. We can configure falco to send the logs to other channels like Slack, PagerDuty, and others.
```yaml
log_stderr: true
log_syslog: true
```
- `log_stderr`: If set to true, falco will send the logs to `stdout`. This is useful for debugging purposes.
- `log_syslog`: If set to true, falco will send the logs to `syslog`. This is useful for production environments where we want to send the logs to a central logging server.

#### Log Level

- The log level for falco. This can be set to `info`, `warning`, `error`, or `debug`. By default, it is set to `info`.
```yaml
log_level: info
```

### Output Channels

- By default, falco will send the alerts to `stdout`. We can configure falco to send the alerts to other channels like Slack, PagerDuty, file systems and others. The output channels are defined in the configuration file under the `output` section.
```yaml
stdout_output:
  enabled: true


file_output:
  enabled: true
  filename: /opt/falco/events.txt


program_output:
  enabled: true
  program: "jq '{text: .output}' | curl -d @- -X POST https://hooks.slack.com/services/XXXX"


http_output:
  enabled: true
  url: http://some.url/some/path/
```

### Priority

```yaml
priority: WARNING
```

This is minimum priority level that falco will log. Any rules that have a lower priority than this will not be logged.

### Hot Reloading

After making changes to the configuration file, we need to reload the falco configuration and restart the engine. We can do this without restarting the entire falco service. This is called hot reloading. We can use the following command to reload the configuration:
```bash
cat /var/run/falco.pid
7183
kill -1 $(cat /var/run/falco.pid)
```

Date of Commit: 20/05/2025