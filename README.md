# tbl-webex-bot

Ansible collection containing the `webex_space` role for Cisco Webex space lifecycle and messaging.

## Collection metadata

- Namespace: `tbl`
- Collection: `webex_bot`
- FQCN: `tbl.webex_bot`
- Role FQCN: `tbl.webex_bot.webex_space`

## Build

```bash
ANSIBLE_LOCAL_TEMP=/tmp/ansible-local ANSIBLE_REMOTE_TEMP=/tmp/ansible-remote \
.venv/bin/ansible-galaxy collection build --output-path /tmp
```

## Install local artifact

```bash
ANSIBLE_LOCAL_TEMP=/tmp/ansible-local ANSIBLE_REMOTE_TEMP=/tmp/ansible-remote \
.venv/bin/ansible-galaxy collection install /tmp/tbl-webex_alerts-0.1.0.tar.gz -p ./collections --force
```

## Use in playbook

```yaml
---
- name: Example
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Create room and post message
      ansible.builtin.include_role:
        name: tbl.webex_bot.webex_space
      vars:
        webex_action: both
        webex_bot_token: "{{ lookup('ansible.builtin.env', 'WEBEX_BOT_TOKEN') }}"
        webex_space_title: "NOC Alerts"
        webex_message: "Webex collection test"
```

## Test playbooks

- Installed collection test: `playbooks/test_webex_space.yml`
