pipeline {
    agent {
        label 'podman'  // Assuming you have a node labeled with 'podman'
    }
    environment {
        CI = 'true'
        PATH = "/usr/local/bin:$PATH"  // Ensure podman is in the PATH
    }
    stages {
        stage('Setup') {
            steps {
                sh 'podman pull node:lts-buster-slim'
                sh '''
                    podman run -d --name nodejs-builder -p 3000:3000 node:lts-buster-slim tail -f /dev/null
                    podman exec nodejs-builder node --version
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
                sh 'podman exec nodejs-builder /bin/sh ./jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh 'podman exec nodejs-builder /bin/sh ./jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh '''
                    podman stop nodejs-builder
                    podman rm nodejs-builder
                '''
            }
        }
    }