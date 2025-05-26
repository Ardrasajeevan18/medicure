pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials' // Replace with your Jenkins DockerHub credentials ID
        DOCKER_IMAGE = "ardra771/project3"
        GIT_REPO = "https://github.com/Ardrasajeevan18/project3.git"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: "${GIT_REPO}"
            }
        }
     stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
       stage('Build') {
           steps {
                sh 'mvn clean package -DskipTests'
                sh 'ls -l target/'   // Check if jar exists here
              }
           }
 
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}") {
                        def customImage = docker.build("${DOCKER_IMAGE}:latest")
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yml'
            }
        }
    }
}
