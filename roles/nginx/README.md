# nginx

Installs nginx and manages its service on Debian-family and RHEL-family systems.

Service state and startup behaviour are fully configurable via defaults.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `nginx_package_name` | `nginx` | Package name to install |
| `nginx_service_name` | `nginx` | Service name to manage |
| `nginx_service_enabled` | `true` | Whether nginx is enabled at boot |
| `nginx_service_state` | `started` | Desired service state |

## Handlers

| Handler | Description |
|---|---|
| `Reload nginx` | Reloads nginx gracefully; use for configuration changes |
| `Restart nginx` | Full restart of nginx; use when reload is insufficient |

## Example usage

```yaml
- name: Install and manage nginx
  hosts: all
  become: true
  roles:
    - role: nginx
```

To stop and disable nginx (e.g. in a test environment):

```yaml
- name: Install and manage nginx
  hosts: all
  become: true
  vars:
    nginx_service_enabled: false
    nginx_service_state: stopped
  roles:
    - role: nginx
```
