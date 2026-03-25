# ansible-demo

Example Ansible playbooks for [Ansilume](https://github.com/ansilume/ansilume).

This repository is automatically added as the **Demo** project in every fresh Ansilume installation. All playbooks are designed to work with real infrastructure — point them at your inventory and they run.

## Playbooks

| Playbook | What it does |
|---|---|
| `upgrade.yml` | Upgrade all installed packages (apt / dnf). Prints a notice if a reboot is required. |
| `install-vim.yml` | Install vim. |
| `install-nginx.yml` | Install nginx, start and enable the service. |
| `install-docker-ce.yml` | Install Docker CE from the official Docker repository (apt / dnf). Optionally adds users to the `docker` group. |
| `install-fail2ban.yml` | Install and configure fail2ban to protect SSH with configurable ban/retry thresholds. |
| `create-user.yml` | Create a system user with optional passwordless sudo and an SSH authorized key. |

## OS support

All playbooks detect the OS family at runtime and use the appropriate package manager:

- **Debian / Ubuntu** — `apt`
- **RHEL / CentOS / Rocky Linux / Fedora** — `dnf`

## Usage in Ansilume

1. Open **Projects → Demo** and click **Sync** to pull the latest playbooks.
2. Go to **Templates** and pick the job template you want to run.
3. Set your inventory and (if needed) SSH credential on the template, then launch.

## Variables

Each playbook documents its variables at the top of the file. Pass them via **Extra Vars** (JSON) in the Ansilume job template or at launch time.

Example for `install-docker-ce.yml`:
```json
{"docker_users": ["ubuntu", "deploy"]}
```

Example for `create-user.yml`:
```json
{"new_user": "deploy", "new_user_sudo": true, "new_user_pubkey": "ssh-ed25519 AAAA..."}
```

## License

[Apache 2.0](LICENSE)
