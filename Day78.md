## Day 78 – Jenkins Conditional Pipeline 🐳⚙️

### 🔎 Why Conditional Pipelines?
Conditional pipelines allow you to run **specific stages only when conditions are met**.  
For example:
- Build only on `main` branch  
- Push Docker images only if the build succeeds  
- Clean workspace at the end  

---

## 🐧 Conditional Docker Build & Push
This example:
- Always clones the repo  
- Builds Docker image only for `dev` and `main` branches  
- Pushes to DockerHub only for the `main` branch  
- Cleans workspace after every build  

```groovy
pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "mydockeruser"
        DOCKER_IMAGE = "myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/myorg/myapp.git'
            }
        }

        stage('Docker Build') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'main'
                }
            }
            steps {
                script {
                    echo "Building Docker image for branch: ${env.BRANCH_NAME} 🐳"
                    docker.build("${DOCKERHUB_USER}/${DOCKER_IMAGE}:${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Push') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "Pushing Docker image to DockerHub 🐳📤"
                    withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
                        docker.image("${DOCKERHUB_USER}/${DOCKER_IMAGE}:${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                              .push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace 🧹"
            cleanWs()
        }
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
```
### Conditional Pipeline for pushing if Docker Build gets successfully then only image will be pushed
```groovy
pipeline {
    agent any

    parameters {
        booleanParam(name: 'PUSH_TO_DOCKERHUB', defaultValue: false, description: 'Push image to DockerHub?')
    }

    environment {
        DOCKERHUB_USER = "mydockeruser"
        DOCKER_IMAGE = "myapp"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/myorg/myapp.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${DOCKERHUB_USER}/${DOCKER_IMAGE}:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Push') {
            when {
                expression { return params.PUSH_TO_DOCKERHUB }
            }
            steps {
                script {
                    withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
                        sh 'docker push ${DOCKERHUB_USER}/${DOCKER_IMAGE}:${BUILD_NUMBER}'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
```
