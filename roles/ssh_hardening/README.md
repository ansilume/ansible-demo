# ssh_hardening

Hardens the OpenSSH server configuration by deploying a drop-in file at
`/etc/ssh/sshd_config.d/99-hardening.conf`. The distro default `sshd_config`
is left untouched; the drop-in overrides the relevant directives because
SSH applies the first matching value it encounters.

The deployed file is validated with `sshd -t` before being placed, and
the service is restarted only on change.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `ssh_port` | `22` | TCP port sshd listens on |
| `ssh_permit_root_login` | `"no"` | `PermitRootLogin` (`no`, `prohibit-password`, `yes`) |
| `ssh_password_authentication` | `false` | Allow password authentication |
| `ssh_pubkey_authentication` | `true` | Allow public key authentication |
| `ssh_permit_empty_passwords` | `false` | Allow empty passwords |
| `ssh_kbd_interactive_authentication` | `false` | Allow keyboard-interactive auth |
| `ssh_kerberos_authentication` | `false` | Allow Kerberos authentication |
| `ssh_gssapi_authentication` | `false` | Allow GSSAPI authentication |
| `ssh_x11_forwarding` | `false` | Allow X11 forwarding |
| `ssh_allow_tcp_forwarding` | `false` | Allow TCP forwarding |
| `ssh_allow_agent_forwarding` | `false` | Allow SSH agent forwarding |
| `ssh_max_auth_tries` | `3` | Max authentication attempts per connection |
| `ssh_login_grace_time` | `30` | Seconds to complete login |
| `ssh_client_alive_interval` | `300` | Keepalive probe interval (seconds) |
| `ssh_client_alive_count_max` | `2` | Missed keepalive probes before disconnect |
| `ssh_allow_users` | `[]` | Whitelist of users (rendered only if non-empty) |
| `ssh_allow_groups` | `[]` | Whitelist of groups |
| `ssh_deny_users` | `[]` | Blacklist of users |
| `ssh_deny_groups` | `[]` | Blacklist of groups |
| `ssh_service_name` | auto | `ssh` on Debian, `sshd` on RHEL |

## Handlers

| Handler | Description |
|---|---|
| `Restart ssh` | Restarts sshd when the drop-in changes |

## Example usage

```yaml
- name: Harden SSH
  hosts: all
  become: true
  roles:
    - role: ssh_hardening
```

Restrict logins to a specific group and move the port:

```yaml
- name: Harden SSH
  hosts: all
  become: true
  vars:
    ssh_port: 2222
    ssh_allow_groups:
      - sshusers
  roles:
    - role: ssh_hardening
```

> Changing `ssh_port` while a firewall is active requires the new port to be
> opened first (see the `firewall` role). Otherwise the restart handler will
> lock you out.
