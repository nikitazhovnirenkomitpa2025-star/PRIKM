pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "nikitazhovnirenko"
        DOCKER_IMAGE       = "${DOCKERHUB_USERNAME}/prikm"
        IMAGE_TAG          = "${env.BUILD_NUMBER}"
    }

    stages {

        stage('Start') {
            steps {
                echo "=== Lab_2: started by GitHub ==="
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Image Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
                sh "docker tag ${DOCKER_IMAGE}:${IMAGE_TAG} ${DOCKER_IMAGE}:latest"
            }
        }

        stage('Push to Docker Hub') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: '3824467',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "# Image successfully pushed to Docker Hub"
            }
        }
    }

    post {

        success {
            echo "# SUCCESS! Image pushed to Docker Hub"
        }

        failure {
            echo "# Pipeline FAILED"
        }
    }
}
