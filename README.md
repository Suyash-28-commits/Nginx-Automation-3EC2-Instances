# Nginx Automation with Ansible on AWS EC2

Automated the installation and startup of **Nginx** on multiple **AWS EC2 Ubuntu instances** using **Ansible**. A local machine acts as the Ansible control node and remotely provisions the target servers over SSH.

## Project Overview

This project demonstrates Infrastructure Automation using Ansible by provisioning three EC2 instances simultaneously.

The playbook performs the following tasks:

- Updates the APT package cache
- Installs Nginx
- Starts the Nginx service
- Logs a confirmation message after successful execution

Since Ansible is **idempotent**, running the playbook multiple times does not reinstall or modify resources that are already in the desired state.

---

## Architecture

```
                  SSH
+------------------------+
|   Laptop (Control Node)|
|       Ansible          |
+-----------+------------+
            |
   -------------------------
   |           |           |
   |           |           |
+------+    +------+    +------+
| EC2  |    | EC2  |    | EC2  |
|Ubuntu|    |Ubuntu|    |Ubuntu|
|Nginx |    |Nginx |    |Nginx |
+------+    +------+    +------+
```

---

## Tech Stack

- Ansible
- AWS EC2
- Ubuntu
- SSH

---

## Project Structure

```
.
├── inventory
├── first-playbook.yml
└── README.md
```

---

## Inventory

```ini
[servers]
ubuntu@<ip_address>
ubuntu@<ip_address>
ubuntu@<ip_address>
```

---

## Playbook

```yaml
---
- name: Install and start nginx on EC2 instance
  hosts: all
  become: true

  tasks:
    - name: Update cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started

    - name: Log that nginx has installed
      shell: echo "Nginx installed and started on EC2 instances"
```

---

## Prerequisites

- Ansible installed on the control node
- Three Ubuntu EC2 instances
- SSH access configured using a private key
- Security Group allowing:
  - SSH (Port 22)
  - HTTP (Port 80)

---

## Running the Playbook

```bash
ansible-playbook -i inventory first-playbook.yml
```

---

## Verify Installation

Check the service status:

```bash
sudo systemctl status nginx
```

Or open the public IP of any EC2 instance in a browser:

```
http://<EC2_PUBLIC_IP>
```

The default Nginx welcome page should be displayed.

---

## Learning Outcomes

- Infrastructure as Code (IaC)
- Configuration Management with Ansible
- Inventory Management
- Remote Server Provisioning
- SSH-based Automation
- Service Management
- Idempotent Playbook Design
- Multi-server Deployment

---

## Future Improvements

- Convert the playbook into an Ansible Role
- Deploy a custom HTML website instead of the default Nginx page
- Use Ansible Variables and Templates
- Configure Load Balancer support
- Integrate with GitHub Actions for CI/CD
- Add Ansible Galaxy role structure

---

## Author

**Suyash Das**
