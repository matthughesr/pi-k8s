# pi-k8s
Ansible playbook to provision a Raspberry Pi 4(4GB ram) with k0s

**Goal:** create an ansible playbook to setup k0s on a raspberry pi. Be able to recover quickly from a complete nuke of the pi

### Why k0s?
- k0s runs standard Kubernetes, mostly unmodified. Other distributions like k3s changes defaults (storage, ingress, networking) to reduce complexity — creates subtle differences from
  "real" Kubernetes.
  - It is more familiar for those with an AWS EKS background.— the YAML, the concepts, and behavior all work the same way. With k3s you'd hit unexpected differences.
  - GitOps friendly — k0s config lives in a YAML file (k0s.yaml) which works well in git.

## Prerequisites
1. Flash Ubuntu Server 22.04 LTS (64-bit, arm64) using Raspberry Pi Imager
    - Set username, password, and enable SSH in the imager before flashing
2. Boot the Pi and confirm you can SSH in: `ssh pimatt@pi.local`
3. Install ansible on your laptop: `pip install ansible`

## What gets setup?
- Common
  - Install base packages
  - Enable swapfile
  - Enable cgroups
  - Set GPU memory split

- Network
  - Setup netplan file for ethernet and wifi(optional)

- k0s
  - Install k0s
  - Setup k0s as a controller
  - Create SAN config file
  - Configure k0s for graceful shutdown

- Kubectl
  - Install kubectl
  - Write kubeconfig


## Secrets setup

Secrets (WiFi SSID and password) are stored in an Ansible vault:

```bash
ansible-vault create ansible/inventories/pi/group_vars/pi/vault.yml
```
Add the following to the vault:
```yaml
vault_host_name: "YourWifiSSID"   # leave empty if ethernet only
vault_wifi_password: "YourWifiPassword"
```

Store your vault password in ~/.vault_pass and add to ansible/ansible.cfg:
```ini
vault_password_file = ~/.vault_pass
```

## Usage
Run the full playbook:
```bash
cd ansible
ansible-playbook site.yml
```

Run a single role:
```bash
ansible-playbook site.yml --tags k0s
```

Dry run (no changes):
```bash
ansible-playbook site.yml --check
```