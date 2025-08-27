## Day 75 – Jenkins Slave Nodes Setup 🖥️⚡

### 🔎 What are Jenkins Slave Nodes?
Jenkins uses a **master-agent architecture**:
- **Master (Controller):** Runs Jenkins UI, schedules jobs.  
- **Slave (Agent):** Executes jobs assigned by the master.  

**Benefits:**
- Distribute workloads  
- Run jobs in different environments (Linux, Windows, Docker)  
- Scale Jenkins pipelines  

---

## 🐧 Step 1: Prepare the Slave Node
- Install **Java** (same version as Jenkins)  
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install openjdk-11-jdk -y

# CentOS/RHEL
sudo yum install java-11-openjdk -y
```
#### Ensure the agent can connect via SSH to the master:
```bash
ssh jenkins@master_ip
```
#### Create a Jenkins user (optional):
```bash
sudo adduser jenkins
sudo passwd jenkins
sudo usermod -aG sudo jenkins
```
Step 2: Configure Slave Node in Jenkins

Go to Manage Jenkins → Manage Nodes and Clouds → New Node

Enter:

Node Name: Linux-Agent

Node Type: Permanent Agent

Click OK

Configure the node:
| Field | Example |
|-------|---------|
| Remote root directory | /home/jenkins |
| Labels | linux docker build |
| Usage | “Use this node as much as possible” |
| Launch method | Launch agents via SSH |

Provide SSH connection details:
| Field | Example |
|-------|---------|
| Host | 192.168.1.100 |
| Credentials | SSH key or user/password |
| Host Key Verification Strategy | Known hosts file Verification Strategy |

Save configuration. Node should show offline initially while connecting.

🐧 Step 3: Verify Connection

Go to Manage Jenkins → Nodes

Node should show online ✅

Step 4: Configure Jobs to Use Slave Node
Freestyle Job

Go to Job → Configure → Restrict where this project can be run

Enter node label: linux

***Pipeline Job***
```groovy
pipeline {
    agent { label 'linux' }
    stages {
        stage('Build') {
            steps {
                echo "Running on Linux slave node 🖥️"
            }
        }
        stage('Test') {
            steps {
                sh 'echo Running tests on slave node'
            }
        }
    }
}
```
