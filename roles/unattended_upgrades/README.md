# unattended_upgrades

Configures automatic security updates. On Debian-family systems this uses
the `unattended-upgrades` package; on RHEL-family systems it uses
`dnf-automatic` with its systemd timer.

A single set of variables drives both backends — set
`unattended_upgrades_security_only: false` to apply all updates instead of
security-only.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `unattended_upgrades_security_only` | `true` | Only apply security updates (maps to `upgrade_type = security` on DNF) |
| `unattended_upgrades_auto_reboot` | `false` | Reboot automatically when kernel/libc updates require it |
| `unattended_upgrades_auto_reboot_time` | `"02:00"` | Time of day to reboot (Debian only) |
| `unattended_upgrades_remove_unused_dependencies` | `true` | Remove unused kernels and dependencies (Debian) |
| `unattended_upgrades_mail` | `""` | Address to receive reports (empty disables mail) |
| `unattended_upgrades_mail_only_on_error` | `true` | Mail only on failure instead of every run |
| `unattended_upgrades_apt_update_interval_days` | `1` | APT cache refresh interval |
| `unattended_upgrades_apt_upgrade_interval_days` | `1` | Unattended upgrade run interval |
| `unattended_upgrades_apt_autoclean_interval_days` | `7` | Autoclean interval |
| `unattended_upgrades_dnf_apply_updates` | `true` | Apply (not just download) updates on RHEL |

## Example usage

```yaml
- name: Configure unattended upgrades
  hosts: all
  become: true
  roles:
    - role: unattended_upgrades
```

Apply all updates (not only security) and reboot when required:

```yaml
- name: Configure unattended upgrades
  hosts: all
  become: true
  vars:
    unattended_upgrades_security_only: false
    unattended_upgrades_auto_reboot: true
    unattended_upgrades_auto_reboot_time: "03:30"
    unattended_upgrades_mail: ops@example.com
  roles:
    - role: unattended_upgrades
```
