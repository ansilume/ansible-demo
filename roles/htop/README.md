# htop

Installs htop on Debian-family and RHEL-family systems.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `htop_package_name` | `htop` | Package name to install |

## Example usage

```yaml
- name: Install htop
  hosts: all
  become: true
  roles:
    - role: htop
```
