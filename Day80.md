## Day 80 – Jenkins Chained Builds 🔗⚡

### 🔎 What are Chained Builds?
**Chained builds** in Jenkins are jobs where the **completion of one job triggers another**.  
They are useful when you want to **break down a pipeline** into smaller jobs:
- Build → Test → Deploy  
- Microservices → Individual jobs triggered in sequence  
- Reducing complexity by separating long pipelines  

---

## 🐧 Example – Chained Builds for a Dockerized App

We’ll set up **two Jenkins jobs**:

1. **Job A – Build & Push Docker Image**  
2. **Job B – Deploy Docker Image to Server**  

---

### 🔹 Job A: Build & Push Docker Image
```groovy
pipeline {
    agent any
    environment {
        DOCKERHUB_USER = "mydockeruser"
        IMAGE_NAME     = "myapp"
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/myorg/myapp.git', branch: 'main'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    echo "🐳 Building Docker image..."
                    docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    echo "📤 Pushing Docker image to DockerHub..."
                    withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                        docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}")
                              .push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build & Push successful!"
            build job: 'Deploy-Job', wait: false, parameters: [
                string(name: 'IMAGE_TAG', value: "${BUILD_NUMBER}")
            ]
        }
    }
}
```
### Job B: Deploy Docker Image to Server
```groovy
pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag to deploy')
    }

    environment {
        DOCKERHUB_USER = "mydockeruser"
        IMAGE_NAME     = "myapp"
        REMOTE_SERVER  = "ubuntu@192.168.1.50"
    }

    stages {
        stage('Deploy to Server') {
            steps {
                script {
                    echo "🚀 Deploying Docker image: ${DOCKERHUB_USER}/${IMAGE_NAME}:${params.IMAGE_TAG}"
                    sshagent (credentials: ['ssh-prod-server']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_SERVER} '
                            docker pull ${DOCKERHUB_USER}/${IMAGE_NAME}:${params.IMAGE_TAG} &&
                            docker stop myapp || true &&
                            docker rm myapp || true &&
                            docker run -d --name myapp -p 8080:8080 ${DOCKERHUB_USER}/${IMAGE_NAME}:${params.IMAGE_TAG}
                        '
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
```
Here
**Job A** builds & pushes image → triggers **Job B** automatically.

build job: step is used to chain jobs.

Parameters **(IMAGE_TAG)** are passed between jobs.

Keeps jobs modular & reusable.
