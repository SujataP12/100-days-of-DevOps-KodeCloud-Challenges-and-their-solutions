## Day 20 – Set Up Jenkins Server ⚙️🤖

### 🔎 What is Jenkins?
**Jenkins** is an open-source automation server used for **CI/CD (Continuous Integration & Continuous Delivery)**.  
It helps automate builds, testing, deployments, and infrastructure tasks.

---

## 🐧 Install Jenkins on Ubuntu/Debian

### 1. Update and Install Dependencies
```bash
sudo apt update -y
sudo apt install openjdk-11-jdk -y
```
### 2. Add Jenkins Repository & Key
```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```
### 3. Install Jenkins
```bash
sudo apt update -y
sudo apt install jenkins -y
```
### 4. Start and Enable Jenkins
```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
## 🐧 Install Jenkins on CentOS/RHEL

### 1. Install Java
```bash
sudo yum install java-11-openjdk -y
```
### 2. Add Jenkins Repository
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
  https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```
### 3. Install Jenkins
```bash
sudo yum install jenkins -y
```
### 4. Start and Enable Jenkins
```
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
## 🌐 Access Jenkins

Open in your browser:
```
http://your_server_ip:8080
```
#### Retrieve Initial Admin Password
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

👉 Paste this password in the Jenkins web interface to unlock.

### ✅ Summary

Jenkins requires Java 11+.

Ubuntu → use apt with Jenkins repo.

CentOS → use yum with Jenkins repo.

Default port: 8080.

#### Setup via browser after installation.

🚀 Now your Jenkins CI/CD Server is ready! 
