pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t remix-app:${BUILD_NUMBER} ."
            }
        }

        stage('Deploy') {
            steps {
                sh "docker ps -q --filter publish=3000 | xargs -r docker stop"
                sh "docker ps -a -q --filter publish=3000 | xargs -r docker rm"
                sh "docker run -d -p 3000:3000 remix-app:${BUILD_NUMBER}"
            }
        }

        stage('Test') {
            steps {
                sh "sleep 5"
                sh "curl http://localhost:3000"
            }
        }
    }

    post {
        always {
            sh "docker rmi remix-app:${BUILD_NUMBER} || true"
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
