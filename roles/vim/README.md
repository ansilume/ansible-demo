# vim

Installs vim on Debian-family and RHEL-family systems.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `vim_package_name` | `vim` | Package name to install |

## Example usage

```yaml
- name: Install vim
  hosts: all
  become: true
  roles:
    - role: vim
```

To install a specific vim variant (e.g. `vim-enhanced` on RHEL):

```yaml
- name: Install vim
  hosts: all
  become: true
  vars:
    vim_package_name: vim-enhanced
  roles:
    - role: vim
```
