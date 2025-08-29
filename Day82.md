# Day 82 – Create Ansible Inventory for App Server Testing (Ubuntu & Windows) ⚡🐧🪟

### 🔎 What is an Ansible Inventory?
An **Ansible inventory** is a file that defines all the **servers (hosts)** Ansible manages.  
It helps in grouping **Ubuntu/Linux** and **Windows** servers for automation and testing.

---

## 🐧 Step 1: Create a Static Inventory File

### Example: `inventory.ini`
```ini
# Ubuntu/Linux App Servers
[app_servers_ubuntu]
app01 ansible_host=192.168.1.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
app02 ansible_host=192.168.1.102 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

# Windows App Servers
[app_servers_windows]
win01 ansible_host=192.168.1.150 ansible_user=Administrator ansible_password=MySecurePass ansible_connection=winrm ansible_winrm_transport=basic ansible_port=5985
win02 ansible_host=192.168.1.151 ansible_user=Administrator ansible_password=MySecurePass ansible_connection=winrm ansible_winrm_transport=basic ansible_port=5985
```

## 🐧 Step 2: Test Connection with Ubuntu Servers
```bash
ansible -i inventory.ini app_servers_ubuntu -m ping
```
## 🪟 Step 3: Test Connection with Windows Servers
```bash
ansible -i inventory.ini app_servers_windows -m win_ping
```
## 🐧 Step 4: YAML-based Inventory (Optional)
### inventory.yaml
```yaml
all:
  children:
    app_servers_ubuntu:
      hosts:
        app01:
          ansible_host: 192.168.1.101
          ansible_user: ubuntu
          ansible_ssh_private_key_file: ~/.ssh/id_rsa
        app02:
          ansible_host: 192.168.1.102
          ansible_user: ubuntu
          ansible_ssh_private_key_file: ~/.ssh/id_rsa

    app_servers_windows:
      hosts:
        win01:
          ansible_host: 192.168.1.150
          ansible_user: Administrator
          ansible_password: MySecurePass
          ansible_connection: winrm
          ansible_winrm_transport: basic
          ansible_port: 5985

```
**Run tests:**
```bash
ansible -i inventory.ini app_servers_ubuntu -m ping

ansible -i inventory.yml app_servers_windows -m win_ping
```
