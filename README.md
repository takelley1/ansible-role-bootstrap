### Common "base" configuration to help bootstrap Unix-based target hosts.

---

What the target host needs before you can run this role against it:
   - Python installed (Most distros come with Python pre-installed).
   - SSHD service running (Most distros enable SSHD by default).
   - Root login allowed in `/etc/sshd_config` (This will be disabled by Ansible after connecting).

You'll also need to add a set of SSH keys for to `keys/service_account/`.
Ansible will use these keys to authenticate to the target host(s) after
running this role successfully. Don't add a password to the key if you
want more convenience when running subsequent playbooks.
```bash
ssh-keygen -f keys/service_account/id_rsa
```

An example playbook using the role:
```yaml
- hosts: foobar
  gather_facts: true
  become: true
  become_method: sudo
  # Change 'ansible' to the value of service_account.username if you changed it.
  remote_user: "{{ user | default('ansible') }}"
  vars:
    # Same here.
    ansible_user: "{{ user | default('ansible') }}"
  roles:
    - ansible-role-bootstrap
```
Using the above playbook, we can connect to our target host using:
```bash
ansible-playbook playbook.yml -e "user=root" -kK
```
Once the role finishes, all subsequent calls can simply be run like this:
```bash
ansible-playbook playbook.yml
```

```
