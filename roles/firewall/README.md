# firewall

Manages a host-based firewall. On Debian-family systems this uses `ufw`
(via `community.general.ufw`); on RHEL-family systems it uses `firewalld`
(via `ansible.posix.firewalld`).

One set of variables drives both backends for port/service rules. Because
ufw and firewalld model rules differently, a few knobs are backend-specific
(`firewalld_zone`, `firewalld_services`, `firewall_default_*`).

> Rules are added **before** the firewall is enabled so you do not lock
> yourself out of the host when running against a remote target. Still make
> sure the SSH port is included in `firewall_allowed_tcp_ports` (default) or
> `firewalld_services` (default `ssh`) before applying.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Variables

| Variable | Default | Description |
|---|---|---|
| `firewall_allowed_tcp_ports` | `[22]` | TCP ports to allow inbound |
| `firewall_allowed_udp_ports` | `[]` | UDP ports to allow inbound |
| `firewall_state` | `enabled` | Final ufw state (`enabled`, `disabled`, `reloaded`, `reset`) |
| `firewall_default_incoming` | `deny` | ufw default policy for incoming traffic |
| `firewall_default_outgoing` | `allow` | ufw default policy for outgoing traffic |
| `firewall_default_routed` | `deny` | ufw default policy for routed traffic |
| `firewall_logging` | `low` | ufw logging level (`off`, `low`, `medium`, `high`, `full`) |
| `firewalld_zone` | `public` | Zone to add rules to on RHEL |
| `firewalld_services` | `[ssh]` | firewalld service names to allow |

## Required collections

- `community.general` (for `ufw`)
- `ansible.posix` (for `firewalld`)

## Example usage

```yaml
- name: Configure host firewall
  hosts: all
  become: true
  roles:
    - role: firewall
```

Open a web server:

```yaml
- name: Configure host firewall
  hosts: web
  become: true
  vars:
    firewall_allowed_tcp_ports:
      - 22
      - 80
      - 443
    firewalld_services:
      - ssh
      - http
      - https
  roles:
    - role: firewall
```
