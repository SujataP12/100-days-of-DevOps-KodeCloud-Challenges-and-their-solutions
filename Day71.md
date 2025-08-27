## Configure Jenkins Job for Package Installation ⚙️📦

### 🔎 Goal
Automate **package installation** on a server using Jenkins.  
This can be used to install **system packages**, **dependencies**, or **application software** on remote servers.

---

## 🐧 Step 1: Create a Jenkins Job
1. Go to Jenkins dashboard → **New Item**  
2. Enter a name (e.g., `InstallPackages`)  
3. Select **Freestyle project** → **OK**

---

## 🐧 Step 2: Configure Source Code (Optional)
If the script is stored in Git:
1. Go to **Source Code Management → Git**  
2. Enter repository URL and credentials  

---

## 🐧 Step 3: Add Build Step
1. Scroll to **Build → Add build step → Execute shell**  
2. Enter the shell commands or script to install packages:

### Example: Ubuntu
```bash
sudo apt update -y
sudo apt install -y git curl vim htop
```
