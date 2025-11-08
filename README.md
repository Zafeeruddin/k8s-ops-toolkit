# k8s-playbooks

Collection of Ansible playbooks to deploy and manage Kubernetes-native applications. Useful for DevOps engineers or SREs looking to automate k8s setups.

## Overview

This repository contains production-ready Ansible playbooks for automating common Kubernetes operations, from cluster setup to application deployment. Each playbook is self-contained and documented for easy integration into your infrastructure automation workflow.

## Prerequisites

- Ansible 2.9 or higher
- SSH access to target nodes
- Python 3.x on target nodes
- Basic understanding of Kubernetes concepts

## Installation

Clone the repository:

```bash
git clone https://github.com/username/k8s-playbooks.git
cd k8s-playbooks
```

Install required Ansible collections (if any):

```bash
ansible-galaxy collection install -r requirements.yml
```

## Playbooks

### `join-worker`
Adds a worker node to an existing Kubernetes cluster. Handles all prerequisites including container runtime setup, kubeadm installation, and cluster join operations.

**Status:** ✅ Available

**Usage:**
```bash
ansible-playbook join-worker/playbook.yml -i inventory.ini
```

### `k8s-master`
Sets up a single-node or multi-master Kubernetes control plane with kubeadm. Includes network plugin installation and cluster initialization.

**Status:** 🚧 Coming Soon

### `k8s-argocd`
Deploys ArgoCD for GitOps-based continuous delivery. Configures ingress, authentication, and initial repository connections.

**Status:** 🚧 Coming Soon

### `k8s-monitoring`
Installs Prometheus and Grafana stack for comprehensive cluster monitoring.

**Status:** 🚧 Coming Soon

### `k8s-backup`
Configures Velero for cluster backup and disaster recovery.

**Status:** 🚧 Coming Soon

## Configuration

Each playbook includes its own `vars/` directory for customization. Copy the example vars file and modify as needed:

```bash
cp playbook-name/vars/main.yml.example playbook-name/vars/main.yml
```

Edit the inventory file to match your infrastructure:

```ini
[k8s_masters]
master1.example.com

[k8s_workers]
worker1.example.com
worker2.example.com
```

## Contributing

We welcome contributions! Here's how to add a new playbook:

1. **Fork the repository**
   ```bash
   git fork https://github.com/username/k8s-playbooks.git
   ```

2. **Create a branch for your playbook**
   ```bash
   git checkout -b playbook/your-feature-name
   ```

3. **Add your playbook with proper structure**
   ```
   your-playbook/
   ├── playbook.yml
   ├── README.md
   ├── vars/
   │   └── main.yml.example
   └── tasks/
       └── main.yml
   ```

4. **Document your playbook**
   - Add a clear README explaining purpose, prerequisites, and usage
   - Include example configurations
   - Document all variables

5. **Test thoroughly**
   - Test on a clean environment
   - Verify idempotency
   - Check for proper error handling

6. **Submit a pull request**
   - Describe what your playbook does
   - Include any testing notes
   - Reference related issues if applicable

## Testing

Run syntax checks before submitting:

```bash
ansible-playbook --syntax-check playbook-name/playbook.yml
```

Use dry-run mode to verify behavior:

```bash
ansible-playbook --check playbook-name/playbook.yml -i inventory.ini
```

## License

MIT License - see [LICENSE](LICENSE) file for details

## Support

- **Issues:** Report bugs or request features via [GitHub Issues](https://github.com/username/k8s-playbooks/issues)
- **Discussions:** Join community discussions in [GitHub Discussions](https://github.com/username/k8s-playbooks/discussions)
- **Documentation:** Check individual playbook READMEs for detailed usage

## Roadmap

- [ ] Add k8s-master playbook
- [ ] Add ArgoCD deployment playbook
- [ ] Add monitoring stack playbook
- [ ] Add automated testing with Molecule
- [ ] Add Helm chart deployment playbook
- [ ] Add cluster upgrade playbook
