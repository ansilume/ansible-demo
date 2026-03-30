# CLAUDE.md

## Purpose

This repository contains production-ready example Ansible playbooks for Ansilume.

The goal is not to create random demos. The goal is to maintain a high-quality catalog of small, readable, reusable, cross-distribution Ansible examples that can be imported into Ansilume projects and safely used as reference implementations.

Claude must treat this repository like an operations repository, not a scratchpad.

---

## Primary Goal

Build and maintain example Ansible content that is:

- production-ready
- lint-clean
- readable by infrastructure teams
- structured around roles, not monolithic playbooks
- compatible with Debian-family and RedHat-family systems
- suitable for Ansilume users as practical examples

Examples include simple packages and services such as:

- vim
- htop
- nginx

But every example must still follow the same structure and quality bar as a real production automation repository.

---

## Non-Negotiable Rules

Claude must always follow these rules:

1. Use `.yaml` only.
   - Never create `.yml` files.
   - If existing `.yml` files are found, prefer migrating them to `.yaml` when safe and when references are updated accordingly.

2. Do not use `.j2` extension on templates.
   - Template files must use their native extension (e.g. `bashrc`, `hosts`, `jail.local`), not `.j2`.
   - Reason: IDEs apply correct syntax highlighting for the actual file type (bash, INI, etc.) when `.j2` is not appended.
   - Ansible resolves templates by the `src:` value in tasks, so the extension has no functional impact.

3. Use roles for implementation.
   - Top-level playbooks may only orchestrate.
   - Real work must live in roles.

4. One playbook per use case.
   - Each top-level playbook should do one thing only.
   - Example: `playbooks/install_vim.yaml` calls role `vim`.
   - Do not create giant “common” playbooks that install many unrelated tools.

5. Every role must have defaults.
   - All configurable values must have sane defaults in `defaults/main.yaml`.
   - Avoid hardcoding values that may differ between environments.

6. Debian and RHEL-family support is mandatory.
   - Support Debian-family systems.
   - Support RHEL-family systems, including AlmaLinux.
   - Claude must use facts and variable maps where needed instead of distro-specific duplication.

7. Fully qualify Ansible modules.
   - Use FQCN such as `ansible.builtin.package`, `ansible.builtin.service`, `ansible.builtin.template`, etc.

8. Idempotency is required.
   - Tasks must be safe to run repeatedly.
   - Avoid shell/command unless there is no proper module.
   - When shell/command is unavoidable, document why and use strict guards.

9. Production linting is required.
   - Repository changes must pass `ansible-lint` with the `production` profile.
   - YAML formatting must pass `yamllint`.
   - GitHub Actions must enforce linting on pushes and pull requests.

10. Keep examples minimal but real.
   - Do not add unnecessary complexity.
   - Do not oversimplify in ways that teach bad habits.

11. Respect existing repository content.
   - Inspect the current structure, existing playbooks, roles, workflows, and docs before changing anything.
   - Prefer improving existing content over duplicating it.

12. Do not introduce local inventory patterns that are not needed.
   - Do not add `inventory/`, `group_vars/`, or similar repository-local inventory scaffolding unless explicitly requested.
   - Assume Ansilume will provide inventory, host targeting, and related runtime context externally.

---

## What Claude Should Optimize For

Optimize in this order:

1. Correctness
2. Idempotency
3. Cross-distro compatibility
4. Lint cleanliness
5. Maintainability
6. Clarity for Ansilume users
7. Simplicity

Do not trade correctness for brevity.

---

## Repository Structure

Claude should keep or migrate the repository toward this structure:

```text
.
├── ansible.cfg
├── .ansible-lint
├── .yamllint
├── playbooks/
│   ├── install_vim.yaml
│   ├── install_htop.yaml
│   └── install_nginx.yaml
├── roles/
│   ├── vim/
│   │   ├── defaults/
│   │   │   └── main.yaml
│   │   ├── handlers/
│   │   │   └── main.yaml
│   │   ├── tasks/
│   │   │   └── main.yaml
│   │   ├── vars/
│   │   │   └── main.yaml
│   │   ├── meta/
│   │   │   └── main.yaml
│   │   └── README.md
│   ├── htop/
│   └── nginx/
├── .github/
│   └── workflows/
│       └── lint.yaml
└── README.md
```

Guidance:

- `playbooks/` contains only orchestration playbooks.
- `roles/` contains implementation.
- `defaults/main.yaml` is the primary configuration surface.
- `vars/main.yaml` should only be used for internal non-overridable mappings.
- `handlers/main.yaml` should exist when service reload/restart may be needed.
- `meta/main.yaml` should declare role metadata and supported platforms where useful.
- Do not add repo-local inventory content unless explicitly requested.

---

## Playbook Rules

Top-level playbooks must be thin.

A top-level playbook should usually:

- define hosts
- optionally define privilege escalation
- optionally expose a small number of role vars
- call exactly one role

Preferred pattern:

```yaml
---
- name: Install vim
  hosts: all
  become: true
  roles:
    - role: vim
```

Avoid putting tasks directly in top-level playbooks unless there is a very strong reason.

---

## Role Rules

Each role must be self-contained and production-usable.

### Required files

Each role should include at least:

- `defaults/main.yaml`
- `tasks/main.yaml`
- `meta/main.yaml`
- `README.md`

Add `handlers/main.yaml` when services are managed.

### Defaults

Defaults must:

- exist for all tunable values
- use explicit namespaced variables
- be safe by default

Example:

```yaml
vim_package_name: vim
```

For nginx:

```yaml
nginx_package_name: nginx
nginx_service_name: nginx
nginx_service_enabled: true
nginx_service_state: started
```

### Variables and OS mapping

Cross-distro differences should be handled cleanly.

Preferred patterns:

- use `ansible_os_family`
- use `ansible_distribution`
- use mapping variables
- avoid duplicating tasks per distro unless behavior is genuinely different

Example:

```yaml
nginx_package_map:
  Debian: nginx
  RedHat: nginx

nginx_service_map:
  Debian: nginx
  RedHat: nginx
```

Then derive internal vars from facts.

### Tasks

Tasks must:

- have clear names
- be idempotent
- use modules instead of shell
- use FQCN
- avoid unnecessary conditionals
- avoid inline complexity when a variable map solves it better

Preferred installation module:

```yaml
ansible.builtin.package
```

Preferred service management:

```yaml
ansible.builtin.service
```

### Handlers

Handlers should only exist when needed.
Use handlers for reload/restart events triggered by changed configuration.

### Metadata

`meta/main.yaml` should describe supported platforms where helpful.

---

## Distribution Support Rules

All examples must work on:

- Debian-family
- RHEL-family
- AlmaLinux

Practical expectations:

- use `ansible.builtin.package` whenever possible
- use variable maps for package/service names
- only use distro-specific modules when strictly necessary
- when distro-specific behavior is required, isolate it clearly and document it

Do not claim support for a distribution unless the role structure actually reflects it.

---

## Linting and Quality Standards

Claude must maintain a strict linting setup.

### ansible-lint

Use a repository configuration that targets production quality.

Required expectations:

- use the `production` profile
- fail the build on lint errors
- prefer fixing lint issues instead of suppressing them
- any ignore entry must be justified in comments or commit message context

### yamllint

Use strict YAML linting with readable line-length settings and standard YAML hygiene.

### Syntax checks

When practical, include lightweight syntax validation for playbooks in CI.

---

## GitHub Actions Requirements

This repository must contain GitHub Actions workflows that run on:

- pull_request
- push to main
- manual dispatch

The workflow must at minimum:

1. check out the repository
2. install a pinned Python version
3. install pinned linting dependencies
4. run `yamllint`
5. run `ansible-lint`
6. fail on any violation

Use stable, pinned action versions where practical.

Do not use floating unstable references for production CI when a stable release tag is available.

---

## Expected Local Tooling

Claude should prefer this toolchain:

- `ansible-core`
- `ansible-lint`
- `yamllint`

If missing, Claude may add:
- `requirements.txt`
- `requirements-dev.txt`
- or another small, clear dependency file

But keep the setup simple.

---

## Documentation Expectations

Every role should have a concise `README.md` that explains:

- what it does
- supported platforms
- default variables
- example usage

The repository `README.md` should explain:

- repository purpose
- structure
- supported systems
- local lint commands
- CI expectations
- how this content fits into Ansilume usage

---

## Refactoring Existing Content

This repository already contains playbooks and possibly workflows.

Claude must:

1. inspect existing files first
2. reuse and improve what is already good
3. migrate inconsistent naming and structure toward this standard
4. avoid duplicate examples
5. keep changes coherent across references and documentation

If an existing playbook contains inline tasks, Claude should usually refactor those tasks into a role.

If an existing file uses `.yml`, Claude should prefer renaming it to `.yaml` and updating references.

---

## What Claude Must Avoid

Claude must not:

- create giant all-in-one playbooks
- duplicate the same logic across multiple playbooks
- use `.yml` for new files
- rely on shell when an Ansible module exists
- add weak examples that teach bad practices
- leave OS support ambiguous
- add unnecessary dependencies
- suppress lint failures without strong justification
- introduce hidden behavior outside defaults/vars
- add repo-local inventory scaffolding unless explicitly requested
- change unrelated parts of the repository

---

## Definition of Done

A task is only done when all of the following are true:

- the playbook is in `.yaml`
- the playbook only orchestrates roles
- the role has sane defaults
- the role is idempotent
- Debian-family support is covered
- RHEL-family support is covered
- AlmaLinux support is covered through the same role logic
- `ansible-lint` passes with strict configuration
- `yamllint` passes
- GitHub Actions workflow exists or is updated
- README documentation is updated if structure or usage changed

---

## Preferred Working Style

When asked to implement changes, Claude should:

1. inspect the current repository layout
2. inspect existing playbooks, roles, and workflows
3. propose the minimum coherent refactor
4. implement the structure cleanly
5. run or prepare lint commands
6. summarize what changed and any follow-up work

Claude should make the repository more consistent with each change.
