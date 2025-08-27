## Day 5 – SELinux Installation and Configuration 🔐🐧

### 🔎 What is SELinux?
**SELinux (Security-Enhanced Linux)** is a Linux kernel security module that provides:
- **Mandatory Access Control (MAC)** — unlike traditional discretionary permissions.
- Fine-grained security policies to control access to files, processes, and system resources.
- Helps prevent privilege escalation and limit damage if a service gets compromised.

👉 In short: **It adds an extra security layer on top of standard Linux permissions.**

---

### 🛠️ Install SELinux (on CentOS / RHEL)
```bash
sudo yum install selinux-policy selinux-policy-targeted policycoreutils -y
```
#### Check SELinux Status
```bash
getenforce
```
Possible outputs:

Enforcing → SELinux is active and enforcing policies

Permissive → SELinux logs policy violations but doesn’t enforce

Disabled → SELinux is turned off

Configure SELinux Mode

##### Edit the configuration file:

```bash
sudo vi /etc/selinux/config
```

##### Set one of the following:
```bash
SELINUX=enforcing    # Enforces security policies
SELINUX=permissive   # Logs policy violations only
SELINUX=disabled     # Completely disables SELinux
```

Save and exit :wq.

🔄 Apply Changes

##### Reboot the system or run:
```bash
sudo setenforce 1   # Switch to enforcing mode (without reboot)
sudo setenforce 0   # Switch to permissive mode (without reboot)
```

### SELinux Installation and Configuration on Ubuntu 🔐🐧

### 🔎 What is SELinux?
**SELinux (Security-Enhanced Linux)** is a kernel security module that enforces **Mandatory Access Control (MAC)** policies.  
It provides fine-grained security rules, restricting processes and users beyond standard Linux permissions.

👉 On Ubuntu, **AppArmor** is the default, but SELinux can be installed and enabled.

---

### 🛠️ Install SELinux on Ubuntu/Debian
```bash
sudo apt update
sudo apt install selinux-basics selinux-policy-default auditd -y
```
Enable SELinux

Initialize SELinux:
```bash
sudo selinux-activate
```
Reboot the system:
```bash
sudo reboot
```
🔎 Check SELinux Status
```bash
getenforce
```

Possible outputs:

Enforcing → SELinux is active and enforcing policies

Permissive → SELinux logs policy violations but doesn’t enforce

Disabled → SELinux is turned off

🔧 Configure SELinux Mode

Edit the configuration file:
```bash
sudo vi /etc/selinux/config
```

Set one of the following:
```bash
SELINUX=enforcing    # Enforce security policies (recommended)
SELINUX=permissive   # Log policy violations only
SELINUX=disabled     # Disable SELinux
```
🔄 Change Mode Without Reboot
```bash
sudo setenforce 1   # Enforcing mode
sudo setenforce 0   # Permissive mode
```

✅ Ubuntu systems ship with AppArmor enabled by default.
If you enable SELinux, you may need to disable AppArmor to avoid conflicts.
```bash
sudo systemctl stop apparmor
sudo systemctl disable apparmor
```
