# debug_vars

**Purpose:** dump all variables visible to the play via `ansible.builtin.debug`
so callers can verify that extra vars, group_vars, host_vars, and role defaults
are passed through cleanly.

This role does not install, configure, or change anything on the target host.

## Supported platforms

Any platform — it only calls `ansible.builtin.debug`.

## Variables

| Variable | Default | Description |
|---|---|---|
| `debug_vars_include_hostvars` | `true` | Also dump `hostvars[inventory_hostname]` |
| `debug_vars_include_facts` | `false` | Also dump `ansible_facts` (requires `gather_facts: true`) |

## Example usage

Dump the play vars:

```yaml
- name: Debug all variables available to the play
  hosts: all
  gather_facts: false
  roles:
    - role: debug_vars
```

Pass arbitrary extra vars and inspect whether they arrive:

```bash
ansible-playbook playbooks/debug_vars.yaml \
  -e foo=bar \
  -e '{"nested": {"key": "value"}}'
```

Include gathered facts in the dump (enable fact gathering in the playbook
and set `debug_vars_include_facts=true`):

```bash
ansible-playbook playbooks/debug_vars.yaml -e debug_vars_include_facts=true
```
