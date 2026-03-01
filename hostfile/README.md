# 📋 Ansible Host Inventory Configuration

This file (`configuration.txt`) is the **Ansible inventory** — it tells Ansible which servers to connect to and how.

## 📁 File

| File                                       | Description                                                   |
| ------------------------------------------ | ------------------------------------------------------------- |
| [`configuration.txt`](./configuration.txt) | Main inventory file with server groups, IPs, and SSH settings |

## 🔧 Sections You Need to Edit

### 1️⃣ Server IPs — Add Your Server Addresses

```ini
# ──────────────────────────────────────────────────
# 👇 EDIT HERE: Replace <server_X_IP> with actual IPs
# ──────────────────────────────────────────────────
[servers]
server_1 ansible_host= <server_1_IP>      # ← Your first server IP (e.g. 54.123.45.67)
server_2 ansible_host= <server_2_IP>      # ← Your second server IP

[prds]
server_3 ansible_host= <server_3_IP>      # ← Your production server IP
```

> **Example:** `server_1 ansible_host=54.210.123.45`

---

### 2️⃣ SSH & Connection Settings

```ini
# ──────────────────────────────────────────────────
# 👇 EDIT HERE: Update these to match your setup
# ──────────────────────────────────────────────────
[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu                                           # ← Your SSH username
ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-key.pem  # ← Path to your SSH private key
```

| Variable                       | What to Set                           | Example                                 |
| ------------------------------ | ------------------------------------- | --------------------------------------- |
| `ansible_user`                 | SSH username for your servers         | `ubuntu`, `ec2-user`, `root`            |
| `ansible_ssh_private_key_file` | Absolute path to your `.pem` key file | `/home/ubuntu/keys/my-key.pem`          |
| `ansible_python_interpreter`   | Path to Python 3 on targets           | `/usr/bin/python3` (usually fine as-is) |

---

## 📖 Understanding Host Groups

| Group        | Purpose                            | Used By Playbooks                             |
| ------------ | ---------------------------------- | --------------------------------------------- |
| `[servers]`  | General-purpose servers            | `date.yml`, `install_nginx.yml`, templates    |
| `[prds]`     | Production servers                 | `deploy_static_web.yml`, `app_deployment.yml` |
| `[all:vars]` | Variables applied to **all** hosts | SSH config, Python path                       |

## ➕ Adding More Servers or Groups

```ini
# Add more servers to an existing group:
[servers]
server_1 ansible_host=54.210.123.45
server_2 ansible_host=54.210.123.46
server_4 ansible_host=54.210.123.47    # ← Just add a new line

# Create a new group:
[staging]
staging_1 ansible_host=10.0.1.50
staging_2 ansible_host=10.0.1.51
```

## ✅ Verify Connectivity

```bash
# Ping all servers
ansible -i configuration.txt all -m ping

# Ping only a specific group
ansible -i configuration.txt servers -m ping
ansible -i configuration.txt prds -m ping

# List all hosts in inventory
ansible-inventory -i configuration.txt --list
```
