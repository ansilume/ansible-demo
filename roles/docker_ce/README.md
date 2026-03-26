# docker_ce

Installs Docker CE from the official Docker repository on Debian-family and RHEL-family systems.
Optionally adds users to the `docker` group.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux (uses the CentOS Docker repository)

## Variables

| Variable | Default | Description |
|---|---|---|
| `docker_ce_users` | `[]` | List of usernames to add to the `docker` group |
| `docker_ce_service_name` | `docker` | Service name to manage |
| `docker_ce_service_enabled` | `true` | Whether Docker is enabled at boot |
| `docker_ce_service_state` | `started` | Desired service state |
| `docker_ce_debian_packages` | see defaults | Packages to install on Debian |
| `docker_ce_redhat_packages` | see defaults | Packages to install on RedHat |

## Example usage

```yaml
- name: Install Docker CE
  hosts: all
  become: true
  roles:
    - role: docker_ce
```

To add users to the docker group:

```yaml
- name: Install Docker CE
  hosts: all
  become: true
  vars:
    docker_ce_users:
      - deploy
      - ci
  roles:
    - role: docker_ce
```
