## Day 7 – Linux SSH Authentication 🔑🐧

### 🎯 Goal
Set up **passwordless SSH authentication** using **SSH key pairs** between two servers (Server A → Server B).  
This improves security and avoids using plain passwords.

---

### 1️⃣ Generate SSH Key (on Client / Server A)
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Press Enter to save in the default location: ~/.ssh/id_rsa

Copy Public Key to Target Server (Server B)
Method 1: Using ssh-copy-id (Recommended)
```bash
ssh-copy-id user@serverB
```
Method 2: Manual (if ssh-copy-id not available)
```bash
cat ~/.ssh/id_rsa.pub | ssh user@serverB "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```
3️⃣ Test SSH Login
```bash
ssh user@serverB
```

👉 You should be logged in without entering a password.

🔧 Configure SSH Daemon (for security)
#### Ubuntu/Debian
```bash
sudo vi /etc/ssh/sshd_config
```
#### CentOS/RHEL
```bash
sudo vi /etc/ssh/sshd_config
```

Update or ensure:
```bash
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart SSH service:

#### Ubuntu/Debian
```bash
sudo systemctl restart ssh
```

#### CentOS/RHEL
```bash
sudo systemctl restart sshd
```

✅ Now SSH authentication is done securely with keys.
This is widely used for 
##### Automation, Ansible, Git, and CI/CD pipelines.
