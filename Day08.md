## Day 8 – Install Ansible ⚙️🤖

### 🔎 What is Ansible?
**Ansible** is an open-source automation tool used for **configuration management, application deployment, orchestration, and IT automation**.  
It uses **YAML playbooks** and works over **SSH (no agents required)**.

---

## 🐧 Install Ansible on Ubuntu/Debian
```bash
# Update packages
sudo apt update -y
# Install Ansible
sudo apt install ansible -y
```
### 👉 Verify installation:
```bash
ansible --version
```
## 🐧 Install Ansible on CentOS/RHEL
```bash
1. Enable EPEL Repository
sudo yum install epel-release -y

2. Install Ansible
sudo yum install ansible -y
```

#### 👉 Verify installation:
```bash
ansible --version
```
