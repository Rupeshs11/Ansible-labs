# 🤖 Ansible Automation Workspace

A hands-on Ansible learning repository — from basic server commands to full application deployments, complete with ready-to-use playbook templates.

## 📁 Project Structure

```
Ansible/
├── 📄 README.md                        ← You are here
├── hostfile/
│   └── configuration.txt              # Ansible inventory (server IPs & SSH config)
├── playbooks/
│   ├── 📄 README.md                    # Playbook documentation & module reference
│   ├── date.yml                       # Check date & uptime on servers
│   ├── install_nginx.yml              # Install & start Nginx
│   ├── deploy_static_web.yml          # Deploy static website with Nginx
│   ├── user_management.yml            # 🔖 Template: User & SSH key management
│   ├── install_docker.yml             # 🔖 Template: Docker & Docker Compose install
│   ├── system_hardening.yml           # 🔖 Template: UFW, SSH hardening, Fail2Ban
│   ├── app_deployment.yml             # 🔖 Template: Git-based app deployment
│   ├── backup_config.yml              # 🔖 Template: Config backup & retention
│   └── monitoring_setup.yml           # 🔖 Template: Node Exporter for Prometheus
└── praticals/
    ├── 📄 README.md                    # Index of all practicals
    └── static-webpage/
        ├── 📄 README.md                # Practical guide & instructions
        ├── index.html                 # Static HTML page to deploy
        └── static-deployment.yml      # Playbook for deployment
```

## 🚀 Getting Started

### Prerequisites

- **Ansible** installed on your control node (`pip install ansible`)
- **SSH key-based access** to your target servers
- Target servers running **Ubuntu/Debian**

### Setup

```bash
# 1. Clone this repository
git clone <your-repo-url>
cd Ansible

# 2. Configure your inventory
#    Edit hostfile/configuration.txt with your server IPs and SSH key path
nano hostfile/configuration.txt

# 3. Test connectivity
ansible -i hostfile/configuration.txt all -m ping

# 4. Run your first playbook
ansible-playbook -i hostfile/configuration.txt playbooks/date.yml
```

## 📚 Quick Links

| Section                                                | Description                                              |
| ------------------------------------------------------ | -------------------------------------------------------- |
| [**Playbooks**](./playbooks/)                          | All playbooks with docs, module reference, and templates |
| [**Practicals**](./praticals/)                         | Hands-on exercises with step-by-step guides              |
| [**Host Configuration**](./hostfile/configuration.txt) | Inventory file with server groups                        |

## 🔖 Template Playbooks

Ready-to-customize blueprints for common DevOps tasks:

| Template                                                   | What It Does                               |
| ---------------------------------------------------------- | ------------------------------------------ |
| [`user_management.yml`](./playbooks/user_management.yml)   | Create users, deploy SSH keys, manage sudo |
| [`install_docker.yml`](./playbooks/install_docker.yml)     | Install Docker & Docker Compose            |
| [`system_hardening.yml`](./playbooks/system_hardening.yml) | Firewall, SSH hardening, Fail2Ban          |
| [`app_deployment.yml`](./playbooks/app_deployment.yml)     | Clone repo, build, deploy, restart service |
| [`backup_config.yml`](./playbooks/backup_config.yml)       | Archive & backup server configs            |
| [`monitoring_setup.yml`](./playbooks/monitoring_setup.yml) | Node Exporter for Prometheus monitoring    |

## 📝 Useful Commands

```bash
# Ping all servers
ansible -i hostfile/configuration.txt all -m ping

# Run a playbook
ansible-playbook -i hostfile/configuration.txt playbooks/<name>.yml

# Dry run (check mode)
ansible-playbook -i hostfile/configuration.txt playbooks/<name>.yml --check

# Run with verbose output
ansible-playbook -i hostfile/configuration.txt playbooks/<name>.yml -vvv

# Limit to specific host
ansible-playbook -i hostfile/configuration.txt playbooks/<name>.yml --limit server_1
```
