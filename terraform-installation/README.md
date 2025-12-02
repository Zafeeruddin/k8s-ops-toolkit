# Terraform Installation with Ansible

This repository contains an Ansible playbook to automate the installation and configuration of Terraform on multiple target hosts at once.

## Overview

This playbook automates the complete setup of Terraform on your target hosts, including:

- Installing required dependencies (unzip, curl, wget)
- Downloading Terraform from the official HashiCorp releases
- Installing Terraform to `/usr/local/bin/terraform`
- Verifying the installation
- Configuring Terraform with optional backend configuration

## Prerequisites

- Python 3.x installed
- `uv` package manager
- SSH access to target hosts
- Sudo/root privileges on target hosts

## Project Setup

1. **Initialize the Python environment:**
   ```bash
   cd terraform-installation
   uv sync
   ```
   This creates a virtual environment and installs all required dependencies (primarily Ansible).

2. **Activate the virtual environment:**
   ```bash
   source .venv/bin/activate
   ```

## Configuration

### Hosts Inventory

Create or edit `ansible/hosts.ini` with your target host details:

```ini
[target_hosts]
192.168.0.146 ansible_user=host1 ansible_password=password ansible_become_pass=password
192.168.0.141 ansible_user=host2 ansible_password=password ansible_become_pass=password
```

Replace the IP addresses and usernames with your actual target host information.

### Secrets Configuration

Create an encrypted secrets file to store sensitive information (if needed):

```bash
ansible-vault create ansible/secret.yml
```

You'll be prompted to create a vault password. Remember this password as you'll need it to run the playbook.

**The `secret.yml` file can contain:**
- Any sensitive configuration values
- API keys or tokens if needed for Terraform providers

### Terraform Configuration

Edit `ansible/configs/configs.yml` to customize Terraform installation:

```yaml
terraform:
  version: "1.6.0"  # Terraform version to install
  install_path: "/usr/local/bin/terraform"
  download_url: "https://releases.hashicorp.com/terraform/{{ terraform.version }}/terraform_{{ terraform.version }}_linux_amd64.zip"
```

## Usage

Run the playbook with:

```bash
ansible-playbook -i ansible/hosts.ini ansible/setup-terraform.yml --ask-vault-pass
```

If you don't have a secrets file, you can run without vault:

```bash
ansible-playbook -i ansible/hosts.ini ansible/setup-terraform.yml
```

**Command breakdown:**
- `-i ansible/hosts.ini`: Specifies the inventory file
- `--ask-vault-pass`: Prompts for the Ansible Vault password to decrypt `secret.yml` (if used)

## What the Playbook Does

1. **Validates Environment**: Checks that required system packages are available

2. **Installs Dependencies**: Installs unzip, curl, and wget if not already present

3. **Downloads Terraform**: Fetches the specified Terraform version from HashiCorp releases

4. **Installs Terraform**: Extracts and installs Terraform to `/usr/local/bin/terraform`

5. **Verifies Installation**: Runs `terraform version` to confirm successful installation

6. **Displays Version**: Shows the installed Terraform version

## Customization

You can modify the following variables in `ansible/configs/configs.yml`:

- `terraform.version`: Terraform version to install (default: latest stable)
- `terraform.install_path`: Installation path for Terraform binary (default: `/usr/local/bin/terraform`)
- `terraform.download_url`: Custom download URL (optional, defaults to HashiCorp releases)

## Troubleshooting

- **Connection refused**: Verify SSH access to target hosts and credentials in `hosts.ini`
- **Permission denied**: Ensure sudo/root access is configured correctly
- **Download failed**: Check network connectivity and HashiCorp releases availability
- **Installation path issues**: Verify the install path is writable and in PATH

## Security Notes

- The `secret.yml` file is encrypted with Ansible Vault for security (if used)
- Never commit unencrypted secrets to version control
- Consider using a secrets management solution for production environments
- Use SSH keys instead of passwords when possible

## Project Structure

```
terraform-installation/
├── ansible/
│   ├── configs/
│   │   └── configs.yml      # Configuration variables
│   ├── hosts.ini            # Inventory file with target host details
│   ├── secret.yml           # Encrypted vault file (optional, create with ansible-vault)
│   └── setup-terraform.yml  # Main playbook
├── pyproject.toml           # Python project configuration
├── uv.lock                  # Dependency lock file
├── .venv/                   # Virtual environment (created by uv sync)
├── COMMANDS.md              # Quick command reference
└── README.md                # This file
```

## Quick Start

```bash
# 1. Setup environment
uv sync
source .venv/bin/activate

# 2. Configure hosts.ini with your target hosts

# 3. (Optional) Create secrets file if needed
ansible-vault create ansible/secret.yml

# 4. Run the playbook
ansible-playbook -K -i ansible/hosts.ini ansible/setup-terraform.yml --ask-vault-pass
```

