# system_user

Creates a system user account with optional passwordless sudo and SSH public key configuration.

`new_user` is required and has no default. The role will fail immediately if it is not set.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `system_user_name` | `""` | **Required.** Username to create |
| `system_user_shell` | `/bin/bash` | Login shell for the user |
| `system_user_sudo` | `false` | Grant passwordless sudo via `/etc/sudoers.d/` |
| `system_user_pubkey` | `""` | SSH public key to add to `authorized_keys` (skipped if empty) |

## Example usage

Minimal (user only):

```yaml
- name: Create system user
  hosts: all
  become: true
  gather_facts: false
  vars:
    system_user_name: deploy
  roles:
    - role: system_user
```

Full (sudo + SSH key):

```yaml
- name: Create system user
  hosts: all
  become: true
  gather_facts: false
  vars:
    system_user_name: deploy
    system_user_sudo: true
    system_user_pubkey: "ssh-ed25519 AAAA..."
  roles:
    - role: system_user
```
