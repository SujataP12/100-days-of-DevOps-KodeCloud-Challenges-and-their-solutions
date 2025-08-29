# Day 81 – Jenkins Multistage Pipeline 🏗️⚡

### 🔎 What is a Multistage Pipeline?
A **multistage pipeline** in Jenkins allows you to **break down your CI/CD flow into multiple stages**, such as:
- **Clone** → **Build** → **Test** → **Docker Build** → **Push** → **Deploy**  
This ensures **clear visibility**, **separation of concerns**, and **faster debugging**.

---

## 🐧 Example – Multistage Pipeline for a Spring Boot App

```groovy
pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "mydockeruser"
        IMAGE_NAME     = "springboot-app"
    }

    stages {
        stage('Clone') {
            steps {
                echo "📥 Cloning repository..."
                git url: 'https://github.com/myorg/springboot-app.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "⚙️ Building application..."
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                sh './mvnw test'
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
                    echo "📤 Pushing image to DockerHub..."
                    withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                        docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "🚀 Deploying to server..."
                    sshagent (credentials: ['ssh-prod-server']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@192.168.1.50 '
                            docker pull ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER} &&
                            docker stop springboot-app || true &&
                            docker rm springboot-app || true &&
                            docker run -d --name springboot-app -p 8080:8080 ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}
                        '
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning up workspace..."
            cleanWs()
        }
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
```
**Stages**: Clone → Build → Test → Docker Build → Push → Deploy

**withDockerRegistry** used for secure DockerHub login

**sshagent** used for secure remote deployment
