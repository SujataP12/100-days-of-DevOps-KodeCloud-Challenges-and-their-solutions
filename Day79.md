## Day 79 – Jenkins Deployment Job 🚀⚡

### 🔎 What is a Deployment Job?
A **deployment job** in Jenkins is used to automatically deliver built artifacts (e.g., Docker images, JAR files) to **staging or production servers**.  
This makes CI/CD pipelines **end-to-end** → from **build** → to **deploy**.

---

## 🐧 Example – Deploy Dockerized App to Remote Server

This pipeline:
- Clones repo  
- Builds Docker image  
- Pushes image to DockerHub  
- Deploys container to a remote server (`prod-server`)  
- Cleans workspace  

```groovy
pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "mydockeruser"
        DOCKER_IMAGE  = "myapp"
        REMOTE_SERVER = "ubuntu@192.168.1.50"
        REMOTE_PATH   = "/opt/myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/myorg/myapp.git', branch: 'main'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    echo "🐳 Building Docker image..."
                    docker.build("${DOCKERHUB_USER}/${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    echo "📤 Pushing Docker image to DockerHub..."
                    withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
                        docker.image("${DOCKERHUB_USER}/${DOCKER_IMAGE}:${BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                script {
                    echo "🚀 Deploying on remote server ${REMOTE_SERVER}..."
                    sshagent (credentials: ['ssh-prod-server']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_SERVER} '
                            docker pull ${DOCKERHUB_USER}/${DOCKER_IMAGE}:latest &&
                            docker stop myapp || true &&
                            docker rm myapp || true &&
                            docker run -d --name myapp -p 8080:8080 ${DOCKERHUB_USER}/${DOCKER_IMAGE}:latest
                        '
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
