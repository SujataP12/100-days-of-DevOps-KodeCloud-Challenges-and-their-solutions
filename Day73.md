## Day 73 – Jenkins Scheduled Jobs ⏰⚡

### 🔎 What are Scheduled Jobs?
Scheduled jobs automate Jenkins tasks using a **cron-like schedule**.  
Common use cases:
- Nightly builds  
- Periodic tests  
- Backups or maintenance scripts  

---

## 🐧 Step 1: Create a Jenkins Job
1. Go to **Jenkins dashboard → New Item**  
2. Enter a name (e.g., `NightlyBuild`)  
3. Choose **Freestyle Project** or **Pipeline** → **OK**  

---

## 🐧 Step 2: Configure Build Trigger

### Freestyle Project
1. Go to **Build Triggers → Build periodically**  
2. Add a cron schedule, e.g., run daily at 2 AM: 0 2 * * *

- Format: `MIN HOUR DOM MON DOW`  
- Example schedules:  
  - `H/15 * * * *` → Every 15 minutes  
  - `0 0 * * 0` → Every Sunday at midnight  

### Pipeline Job
Use the `triggers` block in your Jenkinsfile:
```groovy
pipeline {
    agent any
    triggers {
        cron('0 2 * * *') // Run daily at 2 AM
    }
    stages {
        stage('Scheduled Task') {
            steps {
                echo "Executing scheduled task at ${new Date()}"
            }
        }
    }
}
```
