pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Check Git') {
            steps {
                sh 'git --version'
            }
        }
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                    branches: [[name: 'refs/heads/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/oren1984/myapp.git']]])
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
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image(dockerImage.imageName()).run("-d -p 8080:80")
                }
            }
        }
    }
}
