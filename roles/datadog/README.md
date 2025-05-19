# Datadog Agent Ansible Role

This Ansible role installs and configures the Datadog Agent on multiple platforms:
- Ubuntu/Debian
- RedHat/CentOS/Amazon Linux
- Windows

## Requirements

- Ansible 2.9+
- For Windows hosts: Windows Remote Management (WinRM) configured

## Role Variables

### Required Variables

The Datadog API key should be set as an environment variable in CircleCI:

```yaml
DATADOG_API_KEY: Your Datadog API key
```

### Optional Variables

See `defaults/main.yml` for all default variables:

```yaml
# Datadog site to send data to
datadog_site: "datadoghq.com"

# Optional agent version specification
# datadog_agent_version: "7.36.1"

# Datadog tags to apply to all metrics, logs, and traces
datadog_tags:
  - "env:production"
  - "ansible_managed:true"

# Enable/disable feature flags
datadog_logs_enabled: false
datadog_apm_enabled: false
datadog_process_enabled: false

# Integrations configuration
datadog_checks: {}
```

## Integrations Configuration

You can configure integrations by setting the `datadog_checks` variable:

```yaml
datadog_checks:
  nginx:
    init_config:
    instances:
      - nginx_status_url: http://localhost/nginx_status/
        tags:
          - "service:nginx"
```

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: datadog
      vars:
        datadog_site: "datadoghq.com"
        datadog_logs_enabled: true
        datadog_tags:
          - "env:{{ environment }}"
          - "application:myapp"
        datadog_checks:
          nginx:
            init_config:
            instances:
              - nginx_status_url: http://localhost/nginx_status/
```

## Security Note

This role pulls the Datadog API key from the environment variable, which should be set in CircleCI secrets. Never hardcode the API key in playbooks or templates. 