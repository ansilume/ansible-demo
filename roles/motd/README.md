# motd

Deploys a managed `/etc/motd` that displays hostname, OS, kernel, and IP
at login. Supports optional header and footer text.

> **Note for Ubuntu:** Ubuntu uses `/etc/update-motd.d/` dynamic scripts by
> default. The static `/etc/motd` is still displayed but may appear alongside
> dynamic output. To suppress dynamic MOTD, disable the PAM `pam_motd` module.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `motd_path` | `/etc/motd` | Target path for the MOTD file |
| `motd_header` | `""` | Optional text prepended above the info block |
| `motd_footer` | `""` | Optional text appended below the info block |

## Example usage

```yaml
- name: Deploy message of the day
  hosts: all
  become: true
  vars:
    motd_header: "ACME Corp — Production environment"
  roles:
    - role: motd
```
