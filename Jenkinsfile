pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Setup') {
            steps {
                sh '''
                    podman pull node:lts-buster-slim
                    podman run -d --name nodejs-builder -p 3000:3000 -v $WORKSPACE:/app -w /app node:lts-buster-slim tail -f /dev/null
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'podman exec nodejs-builder npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'podman exec nodejs-builder npm test'
            }
        }
        stage('Deliver') {
            steps {
                sh 'podman exec nodejs-builder npm run build'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh '''
                    podman stop nodejs-builder
                    podman rm nodejs-builder
                '''
            }
        }
    }
}