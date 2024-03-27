pipeline {
    agent any
    
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                   def dockerImageTag = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                    // Build the Docker image
                    def dockerImage = docker.build("M3tina/kiii-jenkins:${dockerImageTag}")
                    // Store the Docker image reference in the 'app' variable
                    app = dockerImage
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    // Use the 'app' variable to push the Docker image
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                        // signal the orchestrator that there is a new version
                    }
                }
            }
        }
    }
}
