# system_upgrade

Upgrades all system packages on Debian-family and RHEL-family systems.

On Debian-family systems, the upgrade type is configurable (`dist` or `safe`). A reboot
notice is printed if `/var/run/reboot-required` is detected after upgrading.

> **Note:** This role does not reboot automatically. Handle reboots separately
> if the debug message indicates one is needed.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `system_upgrade_type` | `dist` | Debian upgrade type: `dist` (full) or `safe` (conservative) |

## Example usage

```yaml
- name: Upgrade system packages
  hosts: all
  become: true
  roles:
    - role: system_upgrade
```

To run a conservative upgrade on Debian:

```yaml
- name: Upgrade system packages
  hosts: all
  become: true
  vars:
    system_upgrade_type: safe
  roles:
    - role: system_upgrade
```
