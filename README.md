# ansible-demo

Production-ready example Ansible playbooks for [Ansilume](https://github.com/ansilume/ansilume).

This repository is automatically added as the **Demo** project in every fresh Ansilume installation. All playbooks are role-based, cross-distribution, idempotent, and pass strict linting.

## Supported systems

- Debian / Ubuntu
- RHEL / AlmaLinux / Rocky Linux / Fedora

## Repository structure

```
.
├── ansible.cfg
├── .ansible-lint
├── .yamllint
├── requirements.yaml
├── requirements-dev.txt
├── playbooks/
│   ├── install_vim.yaml
│   ├── install_htop.yaml
│   ├── install_nginx.yaml
│   ├── install_docker_ce.yaml
│   ├── install_fail2ban.yaml
│   ├── create_user.yaml
│   └── upgrade.yaml
└── roles/
    ├── vim/
    ├── htop/
    ├── nginx/
    ├── docker_ce/
    ├── fail2ban/
    ├── system_user/
    └── system_upgrade/
```

`playbooks/` contains thin orchestration only. All implementation lives in `roles/`.

## Playbooks

| Playbook | Role | Description |
|---|---|---|
| `install_vim.yaml` | `vim` | Install vim |
| `install_htop.yaml` | `htop` | Install htop |
| `install_nginx.yaml` | `nginx` | Install nginx and manage its service |
| `install_docker_ce.yaml` | `docker_ce` | Install Docker CE from the official repository |
| `install_fail2ban.yaml` | `fail2ban` | Install and configure fail2ban with SSH jail |
| `create_user.yaml` | `system_user` | Create a system user with optional sudo and SSH key |
| `upgrade.yaml` | `system_upgrade` | Upgrade all system packages |

## Usage in Ansilume

1. Open **Projects → Demo** and click **Sync** to pull the latest playbooks.
2. Go to **Templates** and pick the job template you want to run.
3. Set your inventory and SSH credential on the template, then launch.

Pass variable overrides via **Extra Vars** (JSON) in the Ansilume job template or at launch time.

**Create a user with sudo and SSH key:**

```json
{"system_user_name": "deploy", "system_user_sudo": true, "system_user_pubkey": "ssh-ed25519 AAAA..."}
```

**Add users to the Docker group:**

```json
{"docker_ce_users": ["ubuntu", "deploy"]}
```

**Conservative package upgrade on Debian:**

```json
{"system_upgrade_type": "safe"}
```

## Local usage

Run against a specific host:

```bash
ansible-playbook -i your_host, playbooks/install_vim.yaml
```

## Linting

Install dependencies:

```bash
pip install -r requirements-dev.txt
ansible-galaxy collection install -r requirements.yaml
```

Run lint checks:

```bash
yamllint .
ansible-lint
```

## CI

GitHub Actions runs `yamllint` and `ansible-lint` on every push to `main` and every pull request.
See [`.github/workflows/lint.yaml`](.github/workflows/lint.yaml).

## License

[Apache 2.0](LICENSE)
