# common

Installs a baseline set of packages and optionally configures SSH authorized
keys for root or other users. Intended as the first role applied to new hosts.

OS-specific package lists are loaded from `vars/debian.yaml` or
`vars/redhat.yaml` at runtime. Additional packages can be injected via
`common_packages`.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `common_packages` | `[]` | Extra packages to install on top of the baseline |
| `common_ssh_keys` | `[]` | SSH authorized keys to add (`user`, `key`) |
| `common_update_cache` | `true` | Update apt cache before installing on Debian |

## Example usage

```yaml
- name: Apply common baseline
  hosts: all
  become: true
  vars:
    common_packages:
      - tmux
      - jq
    common_ssh_keys:
      - user: root
        key: "ssh-ed25519 AAAA..."
  roles:
    - role: common
```
