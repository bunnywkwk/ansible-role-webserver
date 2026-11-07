# ansible-role-webserver

Installs nginx, writes an index page showing a message, opens the port in firewalld and keeps nginx running. It is also the template for new roles.

This repo holds role code only. CI comes from the central pipeline in [ansible-pipeline](https://github.com/bunnywkwk/ansible-pipeline): Jenkins builds any branch that contains `meta/main.yml`.

## Requirements

- Target: RHEL 9 (tested on `ubi9-init`), firewalld and SELinux enforcing supported.
- Control node: ansible-core 2.16 or later, and the collections in `requirements.yml`:

  ```bash
  ansible-galaxy collection install -r requirements.yml
  ```

## Variables

| Variable | Default | Meaning |
| --- | --- | --- |
| `webserver_port` | `80` | Port nginx serves the page on. Another port on SELinux hosts also needs an `http_port_t` label. |
| `webserver_message` | `Hello from the lab` | Text shown on the index page |
| `webserver_manage_firewall` | `true` | Open `webserver_port` in firewalld; set `false` where firewalld does not run |

Types are checked before any task runs (`meta/argument_specs.yml`).

## Example playbook

```yaml
- name: Web servers
  hosts: web
  become: true
  roles:
    - role: webserver
      vars:
        webserver_message: "Hello from production"
```

## Testing

With the CI tools activated (`. /opt/ci/bin/activate`):

```bash
yamllint .
ansible-lint --profile production
/opt/checkov/bin/checkov -d . --framework ansible --skip-download
molecule test        # 1 × ubi9-init with systemd: converge, idempotence, verify
```

## Release

`VERSION` holds the next release. CI tags `v<VERSION>` when `main` is green.

## License

MIT
