pipeline {
    // RUn script on server 1
    agent {
        label 'server1-yaz'
    }

    // Setup tools NodeJs
    tools {
        nodejs 'nodejs'
    }

    // Running CI CD pipeline
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Yaz190/simple-apps.git'
            }
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.10.103:9000 \
                -Dsonar.login=sqp_3fd23bb9169a85f5729cf2a26af6df67754fb7c5'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }
}