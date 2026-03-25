# fail2ban

Installs and configures fail2ban with SSH jail protection on Debian-family and RHEL-family systems.

On RHEL-family systems (excluding Fedora), EPEL is installed automatically to provide the `fail2ban` package.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `fail2ban_bantime` | `600` | Ban duration in seconds |
| `fail2ban_findtime` | `600` | Time window for counting failures (seconds) |
| `fail2ban_maxretry` | `5` | Number of failures before banning |
| `fail2ban_service_name` | `fail2ban` | Service name to manage |
| `fail2ban_service_enabled` | `true` | Whether fail2ban is enabled at boot |
| `fail2ban_service_state` | `started` | Desired service state |

## Handlers

| Handler | Description |
|---|---|
| `Restart fail2ban` | Restarts fail2ban when jail configuration changes |

## Example usage

```yaml
- name: Install and configure fail2ban
  hosts: all
  become: true
  roles:
    - role: fail2ban
```

To harden ban settings:

```yaml
- name: Install and configure fail2ban
  hosts: all
  become: true
  vars:
    fail2ban_bantime: 3600
    fail2ban_maxretry: 3
  roles:
    - role: fail2ban
```
