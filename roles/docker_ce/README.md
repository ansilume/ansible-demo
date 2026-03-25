# docker_ce

Installs Docker CE from the official Docker repository on Debian-family and RHEL-family systems.
Optionally adds users to the `docker` group.

## Supported platforms

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux (uses the CentOS Docker repository)

## Variables

| Variable | Default | Description |
|---|---|---|
| `docker_users` | `[]` | List of usernames to add to the `docker` group |
| `docker_service_name` | `docker` | Service name to manage |
| `docker_service_enabled` | `true` | Whether Docker is enabled at boot |
| `docker_service_state` | `started` | Desired service state |
| `docker_debian_packages` | see defaults | Packages to install on Debian |
| `docker_redhat_packages` | see defaults | Packages to install on RedHat |

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
    docker_users:
      - deploy
      - ci
  roles:
    - role: docker_ce
```
