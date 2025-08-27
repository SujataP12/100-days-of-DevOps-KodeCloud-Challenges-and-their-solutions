# Day 3 – Secure Root SSH Access 🐧

To secure root login over SSH:

### 1. Edit SSH Configuration 🔐
```sh 
sudo vi /etc/ssh/sshd_config
```
### Find and update the following line:
```sh
PermitRootLogin no
```

(This disables direct root login via SSH)
Alternatively, to allow only key-based login for root:
```sh
PermitRootLogin prohibit-password
```
### Restart SSH Service
```sh
sudo systemctl restart sshd
```
### To verify try to login as root via ssh
```sh
ssh root@server_ip
```
If set to no, login will be denied.

If set to prohibit-password, root login is only possible with SSH keys (not passwords).

👉 Best practice: disable root login and use a regular user with sudo privileges.
