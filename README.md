# ansible-role-k3s

Installs a pinned k3s server or agent on Debian trixie and Raspberry Pi OS, with its config and firewall rules.

## What it does

- Checks swap is off, loads `overlay` and `br_netfilter`, and turns on forwarding and iptables filtering of bridged traffic.
- On a Raspberry Pi, appends `cgroup_memory=1 cgroup_enable=memory` to `/boot/firmware/cmdline.txt`, since Pi OS's firmware turns the memory cgroup off. Reboots only if the memory cgroup is still off.
- Opens the firewall with ufw: the Kubernetes API (servers only) from `k3s_api_sources`; the kubelet and flannel's VXLAN from `k3s_node_sources`; traffic routed to and from the pod and service ranges; and pods reaching the node they run on.
- Installs the pinned k3s version with the install script from that version's tag.
- Writes every setting to `/etc/rancher/k3s/config.yaml`, readable by root only, and restarts k3s when it changes. Other roles add drop-ins under `config.yaml.d/`.
  - Servers turn off the bundled Traefik, ServiceLB and local-path storage, encrypt secrets, and add the server's name and address to the API certificate.
  - Servers are tainted `node-role.kubernetes.io/control-plane:NoSchedule`, so only workloads that tolerate it run there. k3s applies the taint when the server first joins.
  - Agents read the join token from the server over SSH without logging it.
- Optionally saves the server's kubeconfig on the machine running Ansible, pointing at the server.

## Variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `k3s_version` | `v1.37.2+k3s1` | k3s release to install |
| `k3s_role` | none, required | `server` or `agent` |
| `k3s_server` | none, required for agents | The server's inventory hostname |
| `k3s_api_sources` | none, required for servers | Addresses allowed to reach the Kubernetes API |
| `k3s_node_sources` | none, required | The cluster's node addresses |
| `k3s_cluster_cidr` | `10.42.0.0/16` | Pod range |
| `k3s_service_cidr` | `10.43.0.0/16` | Service range |
| `k3s_kube_reserved` | `cpu=250m,memory=512Mi` | Kept back from pods for Kubernetes |
| `k3s_system_reserved` | `cpu=250m,memory=512Mi` | Kept back from pods for the OS |
| `k3s_kubeconfig_fetch_path` | empty | Where to save the server's kubeconfig on the machine running Ansible |

## Requirements

ufw installed, and the `ansible.posix` and `community.general` collections from `requirements.yml`.

## Example

```yaml
- name: k3s
  hosts: k3s_cluster
  become: true
  roles:
    - role: tonylea.k3s
```

With `k3s_role`, `k3s_server`, `k3s_api_sources` and `k3s_node_sources` set in the inventory.

## Testing

Molecule starts a server and an agent in systemd containers, installs k3s on both and checks both nodes go `Ready`, along with the kernel command line, settings, firewall rules and the fetched kubeconfig.

## Licence

MIT
