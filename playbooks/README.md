# 📖 Ansible Playbooks

A collection of Ansible playbooks for server automation — from basic system checks to full application deployments.

## 📁 Existing Playbooks

### 1. [`date.yml`](./date.yml) — Date & Uptime Check

> A simple playbook to check the current date and server uptime — perfect for verifying Ansible connectivity.

| Property                 | Value           |
| ------------------------ | --------------- |
| **Target Hosts**         | `servers` group |
| **Privilege Escalation** | No              |
| **Modules Used**         | `command`       |

**What it does:**

- Runs `date` command on all servers in the `servers` group
- Runs `uptime` command to check how long servers have been running

```bash
ansible-playbook -i ../hostfile/configuration.txt date.yml
```

---

### 2. [`install_nginx.yml`](./install_nginx.yml) — Install & Start Nginx

> Installs the latest version of Nginx and ensures it starts on boot.

| Property                 | Value               |
| ------------------------ | ------------------- |
| **Target Hosts**         | `servers` group     |
| **Privilege Escalation** | Yes (`become: yes`) |
| **Modules Used**         | `apt`, `service`    |

**What it does:**

- Installs the latest Nginx package using `apt`
- Starts the Nginx service
- Enables Nginx to start automatically on boot

```bash
ansible-playbook -i ../hostfile/configuration.txt install_nginx.yml
```

---

### 3. [`deploy_static_web.yml`](./deploy_static_web.yml) — Deploy Static Website

> Full deployment pipeline: installs Nginx, starts it, and copies an HTML file to the web root.

| Property                 | Value                    |
| ------------------------ | ------------------------ |
| **Target Hosts**         | `prds` group             |
| **Privilege Escalation** | Yes (`become: yes`)      |
| **Modules Used**         | `apt`, `service`, `copy` |

**What it does:**

- Installs the latest Nginx package
- Starts and enables the Nginx service
- Copies `index.html` from the local machine to `/var/www/html` on the server

```bash
ansible-playbook -i ../hostfile/configuration.txt deploy_static_web.yml
```

> **Note:** Make sure you have an `index.html` file in the same directory or specify the correct `src` path.

---

## 🗂️ Template Playbooks (Blueprints for Future Use)

These are ready-made templates you can customize for your own use cases:

| Template            | File                                             | Description                                       |
| ------------------- | ------------------------------------------------ | ------------------------------------------------- |
| 👤 User Management  | [`user_management.yml`](./user_management.yml)   | Create users, set SSH keys, manage sudo access    |
| 🐳 Docker Install   | [`install_docker.yml`](./install_docker.yml)     | Install Docker & Docker Compose on Ubuntu servers |
| 🔒 System Configure | [`system_configure.yml`](./system_configure.yml) | UFW firewall, SSH hardening, fail2ban setup       |
| 🚀 App Deployment   | [`app_deployment.yml`](./app_deployment.yml)     | Clone repo, build, deploy, restart service        |
| 💾 Backup Config    | [`backup_config.yml`](./backup_config.yml)       | Archive and backup server configs & data          |
| 📊 Monitoring Setup | [`monitoring_setup.yml`](./monitoring_setup.yml) | Install Node Exporter for Prometheus monitoring   |

---

## 📊 Module Quick Reference

| Module           | Purpose                                 | Used In                                      |
| ---------------- | --------------------------------------- | -------------------------------------------- |
| `command`        | Run shell commands                      | `date.yml`                                   |
| `apt`            | Install/remove packages (Debian/Ubuntu) | `install_nginx.yml`, `deploy_static_web.yml` |
| `service`        | Manage system services                  | `install_nginx.yml`, `deploy_static_web.yml` |
| `copy`           | Copy files to remote hosts              | `deploy_static_web.yml`                      |
| `user`           | Manage Linux users                      | `user_management.yml`                        |
| `authorized_key` | Deploy SSH public keys                  | `user_management.yml`                        |
| `apt_repository` | Add APT repositories                    | `install_docker.yml`                         |
| `ufw`            | Manage firewall rules                   | `system_configure.yml`                       |
| `git`            | Clone/pull git repositories             | `app_deployment.yml`                         |
| `archive`        | Create compressed archives              | `backup_config.yml`                          |
| `get_url`        | Download files from URLs                | `monitoring_setup.yml`                       |

## 🚀 Running Playbooks

```bash
# Basic syntax
ansible-playbook -i ../hostfile/configuration.txt <playbook-name>.yml

# Dry run (check mode) — see what would change without making changes
ansible-playbook -i ../hostfile/configuration.txt <playbook-name>.yml --check

# Verbose output for debugging
ansible-playbook -i ../hostfile/configuration.txt <playbook-name>.yml -v

# Limit to specific hosts
ansible-playbook -i ../hostfile/configuration.txt <playbook-name>.yml --limit server_1
```
