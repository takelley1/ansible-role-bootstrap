### Common "base" configuration to help bootstrap target hosts.

---
#### Setup

Target Host:
  - Python installed (Most distros come with Python pre-installed).
  - SSHD service running (Most distros enable SSHD by default).
  - Root login allowed in `/etc/sshd_config` (This will be disabled by Ansible after connecting).

Ansible Server:
  - Copy your Ansible user's public SSH key file to `keys/service_account/`.
  - The corresponding private key must be present in your Ansible user's `~/.ssh` directory.

An example playbook using the role:
```yaml
- hosts: foo
  gather_facts: true
  become: true
  become_method: sudo
  remote_user: "{{ user | default('ansible') }}"
  vars:
    ansible_user: "{{ user | default('ansible') }}"
  roles:
    - ansible-role-bootstrap
```

Using the above playbook, we can connect to our target host using:
```bash
ansible-playbook playbook.yml -e "user=root" -kK
```

Once the role finishes, all subsequent runs can be done like this:
```bash
ansible-playbook playbook.yml
```
