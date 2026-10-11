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

After running the playbook, run these commands from the repository root.
Enter the Pi's sudo password when prompted:

```bash
ssh -t prnvbn@prnvbn-pi.local 'umask 077; sudo sed -E \
  "s/(name|cluster|user|current-context): default$/\1: prnvbn-pi/; s#https://127.0.0.1:6443#https://prnvbn-pi.local:6443#" \
  /etc/rancher/k3s/k3s.yaml > ~/prnvbn-pi.yaml'

(umask 077; mkdir -p kubeconfigs; scp prnvbn@prnvbn-pi.local:prnvbn-pi.yaml kubeconfigs/prnvbn-pi.yaml)
```

Use it:

```bash
kubectl --kubeconfig kubeconfigs/prnvbn-pi.yaml get nodes
```

The file contains admin credentials and is Git-ignored. Its cluster, user, and
context are named `prnvbn-pi`, and its API address is `https://prnvbn-pi.local:6443`.
