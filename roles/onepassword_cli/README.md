# onepassword_cli

Installs the [1Password CLI](https://developer.1password.com/docs/cli/) (`op`)
from the official 1Password package repository.

On Debian-family systems, this adds the APT repository with GPG key verification
and debsig-verify policy. On RedHat-family systems, it adds the YUM/DNF
repository with RPM GPG key import.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `onepassword_cli_package_name` | `1password-cli` | Package name to install |
| `onepassword_cli_debian_prerequisites` | `[gnupg, debsig-verify, python3-debian]` | APT packages installed on Debian-family hosts before configuring the repo |

## Example usage

```yaml
- name: Install 1Password CLI
  hosts: all
  become: true
  roles:
    - role: onepassword_cli
```

After installation, the `op` command is available system-wide. Users authenticate
with `op signin` or by configuring a service account token.
