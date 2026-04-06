# timeout_test

**Purpose:** deliberately long-running no-op role used **exclusively to test
timeout handling** in Ansilume and any surrounding orchestration tooling.

This role does not install, configure, or change anything on the target host.
It simply sleeps for a configurable duration (default: 12 hours) so that you
can verify:

- Ansilume job timeout settings actually abort the run
- SSH / control-persist timeouts behave as expected
- Notification and cleanup paths fire on timeout
- UI / API surfaces correctly reflect a timed-out job

Do **not** use this role in real automation.

## Supported platforms

Any platform — it only calls `ansible.builtin.pause`.

## Variables

| Variable | Default | Description |
|---|---|---|
| `timeout_test_minutes` | `720` | How long to sleep, in minutes (default 12 hours) |

## Example usage

Run with the default 12 hour sleep:

```yaml
- name: Test timeout handling
  hosts: all
  roles:
    - role: timeout_test
```

Override with a shorter duration via extra vars:

```bash
ansible-playbook playbooks/timeout_test.yaml -e timeout_test_minutes=5
```
