# hostsfile

Manages `/etc/hosts` from a template. Always writes the standard loopback
and IPv6 entries for the target host, then appends mandatory and optional
custom entries.

> **Warning:** This role owns the entire `/etc/hosts` file. Any entries not
> defined in the role variables will be removed on the next run.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `hostsfile_servers_mandatory` | `[]` | Host entries always written (raw lines) |
| `hostsfile_servers_optional` | `[]` | Host entries written when provided (raw lines) |
| `hostsfile_unsafe_writes` | `false` | Set `true` in containers where `/etc/hosts` is a bind mount |

## Example usage

```yaml
- name: Manage /etc/hosts
  hosts: all
  become: true
  vars:
    hostsfile_servers_mandatory:
      - "10.0.0.1  gateway gateway.internal"
      - "10.0.0.10 db01 db01.internal"
    hostsfile_servers_optional:
      - "10.0.0.20 monitoring"
  roles:
    - role: hostsfile
```

### Running in containers

When the target is a Docker, Podman, LXC, or Kubernetes container, `/etc/hosts`
is a bind mount and cannot be replaced atomically. Enable `hostsfile_unsafe_writes`
to fall back to in-place writes:

```json
{"hostsfile_unsafe_writes": true}
```
