pipeline {
    agent any
    stages {
        stage('Start') {
            steps {
                echo '=== Lab_2: started by GitHub ==='
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Image Build') {
            steps {
                sh 'docker build -t prikm:latest .'
                sh "docker tag prikm nikitazhovnirenko/prikm:latest"
                sh "docker tag prikm nikitazhovnirenko/prikm:${BUILD_NUMBER}"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '3824467', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push nikitazhovnirenko/prikm:latest'
                    sh "docker push nikitazhovnirenko/prikm:${BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    docker stop my-web || true
                    docker rm my-web || true
                    docker run -d -p 80:80 --name my-web nikitazhovnirenko/prikm:latest
                '''
                echo '#  Application deployed successfully!'
            }
        }
        stage('Post Actions') {
            steps {
                echo '=== Lab_2 completed successfully ==='
            }
        }
    }
}
