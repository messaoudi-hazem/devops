pipeline {
    agent any

    tools {
        maven 'Maven'    
        jdk 'Java17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ayoubchwt/Testing-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
    }
}
