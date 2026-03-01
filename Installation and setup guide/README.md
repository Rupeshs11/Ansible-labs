# 📦 Ansible Installation & Setup Guide

Step-by-step guide to install Ansible and set up your control node to manage remote servers.

> 📄 Detailed notes available in [`Ansible-TWS.pdf`](./Ansible-TWS.pdf)

## 1. Install Ansible (Control Node - Ubuntu)

```bash
# Update packages
sudo apt update

# Install required dependencies
sudo apt install -y software-properties-common

# Add Ansible PPA repository
sudo add-apt-repository --yes --update ppa:ansible/ansible

# Install Ansible
sudo apt install -y ansible

# Verify installation
ansible --version
```

## 2. Set Up SSH Key-Based Authentication

Ansible connects to servers over SSH. You need key-based auth (no passwords).

```bash
# Generate SSH key pair (if you don't have one)
ssh-keygen -t rsa -b 4096 -f ~/keys/ansible-key.pem

# Copy public key to each target server
ssh-copy-id -i ~/keys/ansible-key.pem.pub ubuntu@<server_ip>

# Test SSH connection (should connect without password)
ssh -i ~/keys/ansible-key.pem ubuntu@<server_ip>
```

## 3. Configure Inventory File

The inventory tells Ansible about your servers. See [`../hostfile/configuration.txt`](../hostfile/configuration.txt).

```ini
# Define server groups
[servers]
server_1 ansible_host=<server_1_IP>
server_2 ansible_host=<server_2_IP>

[prds]
server_3 ansible_host=<server_3_IP>

# Connection settings for all hosts
[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-key.pem
```

> 🔗 See the [Hostfile README](../hostfile/README.md) for a full breakdown of each section.

## 4. Test Connectivity

```bash
# Ping all servers
ansible -i ../hostfile/configuration.txt all -m ping

# Expected output:
# server_1 | SUCCESS => { "ping": "pong" }
# server_2 | SUCCESS => { "ping": "pong" }
```

## 5. Run Your First Playbook

```bash
# Check date and uptime on servers
ansible-playbook -i ../hostfile/configuration.txt ../playbooks/date.yml
```

## Common Issues

| Issue                                        | Fix                                                                |
| -------------------------------------------- | ------------------------------------------------------------------ |
| `Permission denied (publickey)`              | Check SSH key path in inventory and ensure key is copied to server |
| `No such file or directory: ansible-key.pem` | Update `ansible_ssh_private_key_file` with correct key path        |
| `python3 not found`                          | Install Python 3 on target: `sudo apt install python3`             |
| `Host unreachable`                           | Check server IP, security group rules, and SSH port (22)           |

## Useful Commands

```bash
# Check Ansible version
ansible --version

# List all hosts in inventory
ansible-inventory -i ../hostfile/configuration.txt --list

# Run ad-hoc command on all servers
ansible -i ../hostfile/configuration.txt all -m shell -a "hostname"

# Dry run a playbook
ansible-playbook -i ../hostfile/configuration.txt ../playbooks/date.yml --check
```
