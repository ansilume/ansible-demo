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
│   ├── bashrc.yaml
│   ├── common.yaml
│   ├── create_user.yaml
│   ├── hostsfile.yaml
│   ├── install_docker_ce.yaml
│   ├── install_fail2ban.yaml
│   ├── install_htop.yaml
│   ├── install_nginx.yaml
│   ├── install_onepassword_cli.yaml
│   ├── install_vim.yaml
│   ├── maintenance.yaml
│   ├── motd.yaml
│   └── upgrade.yaml
└── roles/
    ├── bashrc/
    ├── common/
    ├── docker_ce/
    ├── fail2ban/
    ├── hostsfile/
    ├── htop/
    ├── maintenance/
    ├── motd/
    ├── nginx/
    ├── onepassword_cli/
    ├── system_upgrade/
    ├── system_user/
    └── vim/
```

`playbooks/` contains thin orchestration only. All implementation lives in `roles/`.

## Playbooks

| Playbook | Role | Description |
|---|---|---|
| `bashrc.yaml` | `bashrc` | Deploy a managed `.bashrc` to one or more users |
| `common.yaml` | `common` | Install baseline packages and configure SSH keys |
| `create_user.yaml` | `system_user` | Create a system user with optional sudo and SSH key |
| `hostsfile.yaml` | `hostsfile` | Manage `/etc/hosts` |
| `install_docker_ce.yaml` | `docker_ce` | Install Docker CE from the official repository |
| `install_fail2ban.yaml` | `fail2ban` | Install and configure fail2ban with SSH jail |
| `install_htop.yaml` | `htop` | Install htop |
| `install_nginx.yaml` | `nginx` | Install nginx and manage its service |
| `install_onepassword_cli.yaml` | `onepassword_cli` | Install the 1Password CLI from the official repository |
| `install_vim.yaml` | `vim` | Install vim |
| `maintenance.yaml` | `maintenance` | Remove unused packages and clean caches |
| `motd.yaml` | `motd` | Deploy a managed message of the day |
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

**Add SSH keys and extra packages via common:**

```json
{"common_ssh_keys": [{"user": "root", "key": "ssh-ed25519 AAAA..."}], "common_packages": ["tmux"]}
```

**Custom /etc/hosts entries:**

```json
{"hostsfile_servers_mandatory": ["10.0.0.10  db01 db01.internal"]}
```

## Local usage

Run against a specific host:

```bash
ansible-playbook -i your_host, playbooks/common.yaml
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
