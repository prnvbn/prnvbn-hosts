# prnvbn-hosts

Ansible playbooks for my hosts.

Install Ansible:

```bash
brew install ansible
```

run the following to push changes to the pi

```bash
ansible-playbook playbooks/prnvbn-pi.yml --ask-become-pass
```
