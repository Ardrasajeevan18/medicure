pipeline {
    agent any

    environment {
        IMAGE_NAME = "ardra771/project3"
        REGISTRY_CREDENTIALS = 'dockerhub-credentials' // Add this in Jenkins credentials
    }

    tools {
        maven 'Maven 3' // Make sure this tool is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ardrasajeevan18/project3.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIALS) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl set image deployment/project3-deployment project3=ardra771/project3:${BUILD_NUMBER} --record
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build, Docker push, and deploy completed!'
        }
        failure {
            echo '❌ Build failed. Check errors above.'
        }
    }
}
