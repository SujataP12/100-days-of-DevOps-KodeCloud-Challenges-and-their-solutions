## Day 21 – Install Jenkins Plugins 🧩⚙️

### 🔎 What are Jenkins Plugins?
**Plugins** extend Jenkins functionality for CI/CD pipelines, integrations, and automation.  
They allow Jenkins to work with tools like **Git, Docker, Kubernetes, Ansible, SonarQube**, etc.  

---

## 🐧 Installing Jenkins Plugins

Jenkins plugins can be installed in **two ways**:

### 1️⃣ Install via Jenkins Web UI
1. Go to Jenkins dashboard → **Manage Jenkins**  
2. Select **Plugins → Available Plugins**  
3. Search for a plugin → Check the box → **Install without restart**  

### 2️⃣ Install via Jenkins CLI
```bash
# Example: Install Git plugin
java -jar jenkins-cli.jar -s http://localhost:8080/ \
  install-plugin git -deploy
```
## Essential Jenkins Plugins for DevOps CI/CD

| Category            | Plugin Name                                          | Purpose 🚀                 |
| ------------------- | ---------------------------------------------------- | -------------------------- |
| **Source Code**     | Git, GitHub, GitLab                                  | SCM integration            |
| **Build Tools**     | Maven Integration, Gradle                            | Java/Gradle builds         |
| **Pipeline**        | Pipeline, Pipeline: Stage View, Declarative Pipeline | CI/CD pipeline jobs        |
| **Containers**      | Docker, Docker Pipeline, Kubernetes                  | Docker & K8s integration   |
| **Code Quality**    | SonarQube Scanner, Warnings Next Generation          | Code analysis & reports    |
| **Notifications**   | Slack, Email Extension                               | Send alerts                |
| **Security**        | Credentials Binding, Role-based Authorization        | Secure credentials & RBAC  |
| **UI Enhancements** | Blue Ocean                                           | Modern pipeline UI         |
| **Others**          | SSH Agent, AnsiColor                                 | Remote access & color logs |

### Verify Installed Plugins

From Jenkins UI:

#### Go to Manage Jenkins → Plugins → Installed

Or from CLI:
```bash
java -jar jenkins-cli.jar -s http://localhost:8080/ list-plugins
```
