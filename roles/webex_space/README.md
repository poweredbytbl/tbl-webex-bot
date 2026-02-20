# webex_space role

Creates (or reuses) a Cisco Webex space, invites participants, and posts messages.

## Collections

Install project collections with:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

## Variables

- `webex_bot_token` (required): Bot token for Webex API.
- `webex_action`: `create`, `message`, or `both` (default: `both`).
- `webex_space_title`: Space title to find/create.
- `webex_space_id`: Existing space ID (if known).
- `webex_reuse_existing_space`: Reuse exact-title match before creating (default: `true`).
- `webex_participants`: List of email addresses to invite.
- `webex_message`: Single plain text message.
- `webex_markdown`: Single markdown message.
- `webex_messages`: List of message objects, each with `text` and/or `markdown`.
- `webex_api_base_url`: Default `https://webexapis.com/v1`.
- `webex_validate_certs`: Default `true`.
- `webex_timeout`: Default `30`.

## Usage

### 1) Create/reuse space and add participants

```yaml
- name: Ensure Webex space exists and has participants
  ansible.builtin.include_role:
    name: webex_space
    tasks_from: create
  vars:
    webex_bot_token: "{{ vault_webex_bot_token }}"
    webex_space_title: "NOC Alerts"
    webex_reuse_existing_space: true
    webex_participants:
      - alice@example.com
      - bob@example.com
```

After this task, `webex_space_id` is set and can be reused.

### 2) Post one message

```yaml
- name: Send alert to Webex
  ansible.builtin.include_role:
    name: webex_space
    tasks_from: message
  vars:
    webex_bot_token: "{{ vault_webex_bot_token }}"
    webex_space_id: "{{ webex_space_id }}"
    webex_message: "Build failed on {{ inventory_hostname }}"
```

### 3) Post multiple messages

```yaml
- name: Send multiple updates
  ansible.builtin.include_role:
    name: webex_space
    tasks_from: message
  vars:
    webex_bot_token: "{{ vault_webex_bot_token }}"
    webex_space_id: "{{ webex_space_id }}"
    webex_messages:
      - text: "Deployment started"
      - markdown: "**Deployment finished**"
```

### 4) Single-call flow (create + message)

```yaml
- name: Create room and post in one call
  ansible.builtin.include_role:
    name: webex_space
  vars:
    webex_action: both
    webex_bot_token: "{{ vault_webex_bot_token }}"
    webex_space_title: "NOC Alerts"
    webex_participants:
      - alice@example.com
    webex_markdown: "**Job complete**"
```

## Test Playbook

Use the included playbook at `playbooks/test_webex_space.yml` to run an end-to-end check.

```bash
export WEBEX_BOT_TOKEN="your_token"
export WEBEX_SPACE_TITLE="NOC Alerts"
export WEBEX_PARTICIPANTS="alice@example.com,bob@example.com"
export WEBEX_TEST_MESSAGE="Role test from Ansible"

ANSIBLE_CONFIG=ansible.cfg ansible-playbook playbooks/test_webex_space.yml
```
