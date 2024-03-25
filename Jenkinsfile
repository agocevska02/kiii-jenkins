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
                    def app = docker.build("agocevska02/kiii-jenkins")
                    return app // Return app to make it accessible in subsequent stages
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    def app = sh(script: 'echo $app', returnStdout: true).trim() // Retrieve app from previous stage
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        sh "docker push agocevska02/kiii-jenkins:${env.BRANCH_NAME}-${env.BUILD_NUMBER}" // Push with correct tag
                        sh "docker push agocevska02/kiii-jenkins:${env.BRANCH_NAME}-latest" // Push with correct tag
                        // signal the orchestrator that there is a new version
                    }
                }
            }
        }
    }
}
