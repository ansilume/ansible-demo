# maintenance

Removes unused packages and cleans package caches. Safe to run on a
schedule or after upgrades.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `maintenance_autoremove` | `true` | Remove orphaned dependency packages |
| `maintenance_autoclean` | `true` | Remove obsolete cached packages (Debian only) |

## Example usage

```yaml
- name: Run system maintenance
  hosts: all
  become: true
  roles:
    - role: maintenance
```
