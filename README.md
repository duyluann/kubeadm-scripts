# Kubeadm Cluster Setup Scripts

This project provides automated scripts to set up a Kubernetes cluster using kubeadm. It creates a 3-node cluster (1 master and 2 workers) using Vagrant and VirtualBox.

## Cluster Architecture

- 1 Master Node
  - Hostname: master-node
  - IP: 10.0.0.10
  - Resources: 4GB RAM, 2 CPUs
- 2 Worker Nodes
  - Hostnames: worker-node01, worker-node02
  - IPs: 10.0.0.11, 10.0.0.12
  - Resources: 2GB RAM, 1 CPU each

## Prerequisites

- Vagrant
- VirtualBox
- At least 8GB of free RAM (4GB for master + 2GB per worker)
- Sufficient disk space for VMs

## Project Structure

```
.
├── Vagrantfile          # VM configuration and provisioning
├── scripts/
│   ├── common.sh       # Common setup for all nodes
│   └── master.sh       # Master node specific setup
├── manifests/          # Kubernetes manifests
└── terraform/          # Infrastructure as Code (optional)
```

## Getting Started

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd kubeadm-scripts
   ```

2. Start the VMs:
   ```bash
   vagrant up
   ```

3. Check VM status:
   ```bash
   vagrant status
   ```

4. Access the master node:
   ```bash
   vagrant ssh master
   ```

## Available Scripts

- `common.sh`: Installs Docker, kubeadm, kubectl, and kubelet on all nodes
- `master.sh`: Initializes the Kubernetes cluster and sets up the control plane

## Post-Installation

After the VMs are up and running:
1. The master node will be automatically initialized
2. Worker nodes will be ready to join the cluster
3. You can find the join command in the master node's output or by running:
   ```bash
   kubeadm token create --print-join-command
   ```

## Troubleshooting

- If you encounter any issues during setup, try:
  ```bash
  vagrant destroy -f
  vagrant up
  ```
- Check the logs in `/var/log/` on each node for detailed error messages

## Cleanup

To destroy all VMs and clean up:
```bash
vagrant destroy -f
```
