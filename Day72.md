## Jenkins Parameterized Builds 🛠️⚡

### 🔎 What are Parameterized Builds?
Parameterized builds allow **passing inputs (parameters)** to a Jenkins job at runtime.  
This is useful for:
- Deploying to different environments (dev, staging, prod)  
- Passing version numbers, branch names, or configuration options  

---

## 🐧 Step 1: Create a Parameterized Jenkins Pipeline
1. Go to Jenkins dashboard → **New Item**  
2. Enter a name (e.g., `DeployAppPipeline`)  
3. Select **Pipeline** → **OK**  

---

## 🐧 Step 2: Enable Parameters
1. In the pipeline configuration, check **This project is parameterized**  
2. Add parameters:

| Parameter Type | Name           | Default Value | Description                |
|----------------|----------------|---------------|----------------------------|
| String         | ENVIRONMENT    | dev           | Target environment         |
| String         | VERSION        | 1.0.0         | App version to deploy      |
| Choice         | SERVER         | server1       | Select target server       |

---

## 🐧 Step 3: Pipeline Script Example (Declarative)
```groovy
pipeline {
    agent any

    parameters {
        string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Target environment')
        string(name: 'VERSION', defaultValue: '1.0.0', description: 'Application version')
        choice(name: 'SERVER', choices: ['server1','server2'], description: 'Select server')
    }

    stages {
        stage('Print Parameters') {
            steps {
                echo "Deploying version: ${params.VERSION} to environment: ${params.ENVIRONMENT} on server: ${params.SERVER}"
            }
        }

        stage('Install Packages') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'dev') {
                        sh 'sudo apt update -y && sudo apt install -y git curl'
                    } else {
                        sh 'sudo yum update -y && sudo yum install -y git curl'
                    }
                }
            }
        }

        stage('Deploy App') {
            steps {
                echo "Deploying application version ${params.VERSION} to ${params.SERVER}"
                // Add your deployment commands here
            }
        }
    }
}
```
