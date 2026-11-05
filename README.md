# prnvbn-hosts

Ansible playbooks for my hosts.

Install Ansible:

```bash
brew install ansible
```

Run setup:

```bash
ansible-playbook playbooks/prnvbn-pi.yml --ask-become-pass
```

Add the Pi to your local kubeconfig after setup. Install `kubectl` with
`brew install kubectl`, then run:

```bash
ansible-playbook playbooks/kubeconfig.yml --ask-become-pass
kubectl --context prnvbn-pi get nodes
```

Enter the Pi's sudo password when prompted. The command adds or refreshes the
`prnvbn-pi` cluster, user, and context in `$KUBECONFIG` or `~/.kube/config`, preserving
other entries and your current context. It saves the previous file as `.bak`
beside the destination. Both files contain admin credentials and have mode `0600`.
Run it again to refresh the Pi's credentials.

To choose a destination explicitly:

```bash
ansible-playbook playbooks/kubeconfig.yml --ask-become-pass \
  -e kubeconfig_path=/path/to/config
```

If `KUBECONFIG` lists multiple files, pass the single file you want to update explicitly.

Ansible fetches the credentials, then runs `kubectl config set-cluster`,
`set-credentials`, and `set-context` on your computer against a temporary copy of
your config. It embeds the certificates, saves the result, and removes temporary
credentials. It does not select a default context, including for a new config.

To make the Pi your default cluster:

```bash
kubectl config use-context prnvbn-pi
```
