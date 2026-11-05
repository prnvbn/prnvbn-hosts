# k3s role

The `k3s` role installs a single-node K3s server, enables its systemd service,
and waits for the Kubernetes API to become ready. It defaults to `v1.37.1+k3s1`;
override `k3s_version` in inventory or with `--extra-vars k3s_version=VERSION`.
The role enables memory cgroups in the Pi's boot configuration and reboots if
memory cgroups are disabled. The Pi needs at least 2 CPU cores and 2 GB of RAM.

Check the cluster on the Pi:

```bash
sudo k3s kubectl get nodes
```

The admin kubeconfig is `/etc/rancher/k3s/k3s.yaml`, readable by root. K3s includes
CoreDNS for cluster DNS.

## Generate the local kubeconfig

After running the playbook, run this command from the repository root.
It requires local Ansible and `kubectl`. Enter the Pi's sudo password when prompted:

```bash
ansible-playbook playbooks/kubeconfig.yml --ask-become-pass
```

Use it:

```bash
kubectl --context prnvbn-pi get nodes
```

The command merges the admin credentials into your existing kubeconfig, preserving
other clusters and your current context. Its cluster, user, and context are named
`prnvbn-pi`, and its API address uses the inventory's `ansible_host` on port 6443.
See the root README for destination and backup details.
