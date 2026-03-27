# bashrc

Deploys a managed `.bashrc` to one or more users. Supports custom aliases,
exports, and history settings.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `bashrc_users` | `[{name: root, home: /root}]` | Users to receive the `.bashrc` (optional `group` key, defaults to `name`) |
| `bashrc_history_size` | `10000` | `HISTSIZE` value |
| `bashrc_history_filesize` | `20000` | `HISTFILESIZE` value |
| `bashrc_aliases` | `[]` | Additional aliases `{name, command}` |
| `bashrc_exports` | `[]` | Additional `export KEY=VALUE` lines |

## Example usage

```yaml
- name: Deploy .bashrc
  hosts: all
  become: true
  vars:
    bashrc_aliases:
      - name: k
        command: kubectl
    bashrc_exports:
      - EDITOR=vim
  roles:
    - role: bashrc
```
