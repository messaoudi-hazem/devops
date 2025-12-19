pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "eclipse002/spring-app:latest"
        KUBE_NAMESPACE = "devops"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/spring-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    script {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl set image deployment/spring-app spring-app=${DOCKER_IMAGE} -n ${KUBE_NAMESPACE}"
                    sh "kubectl rollout status deployment/spring-app -n ${KUBE_NAMESPACE}"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Something went wrong..."
        }
    }
}
