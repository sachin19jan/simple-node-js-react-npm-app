pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'podman pull node:lts-buster-slim'
                }
            }
        }
    }
}