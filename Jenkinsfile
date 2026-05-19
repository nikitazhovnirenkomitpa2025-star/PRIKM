pipeline {
    agent any
    stages {
        stage('Start') {
            steps {
                echo 'Lab_1: nginx/custom'
            }
        }
        stage('Build nginx/custom') {
            steps {
                sh 'docker build -t nginx/custom:latest .'
            }
        }
        stage('Test nginx/custom') {
            steps {
                echo 'Pass'
            }
        }
        stage('Deploy nginx/custom'){
            steps {
                sh '''
                    docker stop my-web || true
                    docker rm my-web || true
                    docker run -d -p 80:80 --name my-web nginx/custom:latest
                '''
            }
        }
    }
}
