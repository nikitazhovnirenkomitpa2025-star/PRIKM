pipeline {

    agent any

    stages {

        stage('Start') {

            steps {

                echo '=== ########### ###### #1 - ####### ==='

            }

        }

        stage('Checkout') {

            steps {

                git branch: 'Lab_1', url: 'https://github.com/nikitazhovnirenkomitpa2025-star/PRIKM.git'

            }

        }

        stage('Build Docker Image') {

            steps {

                sh 'docker build -t nginx/custom:latest .'

            }

        }

        stage('Test') {

            steps {

                echo '########## ######## #######'

                sh 'docker images | grep nginx/custom'

            }

        }

        stage('Deploy') {

            steps {

                sh '''

                    docker stop my-web || true

                    docker rm my-web || true

                    docker run -d -p 80:80 --name my-web nginx/custom:latest

                '''

                echo '######### ####### ######## ## ##### 80'

            }

        }

        stage('Post Actions') {

            steps {

                echo '=== ########### ###### #1 ######### ==='

            }

        }

    }

}
