# Day 4 – Script Execution Permissions 📝⚡

I am writing a script to welcome the people in this tutorial and make it executable:

### 1. Create a Script
```bash
vi welcome.sh
```

Then write the following content like this
```bash
#!/bin/bash
echo "👋 Welcome to the DevOps Tutorial 🚀"
echo "🐧 You will learn Linux, Docker, Kubernetes, Ansible, Terraform, Jenkins, and AWS ☁️"
```
To save the file, press esc, and then press :wq to exit safely

To give the file executable permissions do this: 
```bash
chmod +x welcome.sh
```
And to run the script do this:
```bash
./welcome.sh
```
## Output will look like this :
```bash
👋 Welcome to the DevOps Tutorial 🚀
🐧 You will learn Linux, Docker, Kubernetes, Ansible, Terraform, Jenkins, and AWS ☁️
```
