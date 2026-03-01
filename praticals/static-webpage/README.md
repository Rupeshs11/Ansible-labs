# 🌐 Practical: Static Webpage Deployment with Ansible

Deploy a static HTML webpage to remote servers using **Nginx** and **Ansible** — fully automated, no manual SSH required.

## 📋 What This Practical Covers

- Installing Nginx on target servers using the `apt` module
- Starting and enabling Nginx as a system service
- Copying a local HTML file to the web server's document root
- Using `become: yes` for privilege escalation

## 📁 Files

| File                                               | Description                                                                                      |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| [`static-deployment.yml`](./static-deployment.yml) | Ansible playbook that installs Nginx, starts it, and deploys the `index.html` to `/var/www/html` |
| [`index.html`](./index.html)                       | The static HTML page to be deployed (customize with your own content)                            |

## 🔧 Prerequisites

- Ansible installed on the control node
- SSH access to target servers (key-based authentication)
- Inventory file configured with a `[prds]` host group (see [`../../hostfile/configuration.txt`](../../hostfile/configuration.txt))
- Target servers running Ubuntu/Debian (uses `apt` module)

## 🚀 How to Run

```bash
# 1. Make sure your inventory is configured
#    Edit ../../hostfile/configuration.txt with your server IPs

# 2. Add your content to index.html
#    Replace the empty file with your actual HTML

# 3. Run the playbook
ansible-playbook -i ../../hostfile/configuration.txt static-deployment.yml
```

## 📝 Playbook Breakdown

```yaml
- name: Install nginx and serve static website
  hosts: prds # Targets the 'prds' host group
  become: yes # Run tasks with sudo privileges
  tasks:
    - name: Install nginx # Uses apt module to install latest Nginx
    - name: Start nginx # Starts service & enables it on boot
    - name: Deploy web page # Copies index.html to /var/www/html
```

## ✅ Verify Deployment

After running the playbook, open your browser and navigate to:

```
http://<your-server-ip>
```

You should see your static webpage served by Nginx.

## 💡 Key Ansible Concepts Used

| Concept              | Module/Keyword | Purpose                                       |
| -------------------- | -------------- | --------------------------------------------- |
| Package Management   | `apt`          | Install software packages                     |
| Service Management   | `service`      | Start/stop/enable system services             |
| File Transfer        | `copy`         | Copy files from control node to managed nodes |
| Privilege Escalation | `become: yes`  | Run tasks as root/sudo                        |
