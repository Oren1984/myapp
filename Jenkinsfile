pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "myapp:${env.BUILD_NUMBER}"
                    dockerImage = docker.build(imageTag)
                }
            }
        }
        stage('Stop Previous Container') {
            steps {
                script {
                    sh """
                        docker ps -q --filter "ancestor=myapp:${env.BUILD_NUMBER}" | xargs -r docker stop
                        docker ps -aq --filter "ancestor=myapp:${env.BUILD_NUMBER}" | xargs -r docker rm
                    """
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image(dockerImage.imageName()).run("-d -p 8080:80")
                }
            }
        }
    }
}
