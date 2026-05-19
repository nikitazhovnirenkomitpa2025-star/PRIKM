pipeline {

    agent any

    stages {

        stage('Start') {

            steps {

                echo 'Lab_2: started by GitHub'

            }

        }

        stage('Image build') {

            steps {

                sh "docker build -t prikm:latest ."

                sh "docker tag prikm nikitazhovnirenko/prikm:latest"

                sh "docker tag prikm nikitazhovnirenko/prikm:\${BUILD_NUMBER}"

            }

        }

        stage('Push to registry') {

            steps {

                withDockerRegistry([ credentialsId: '3824467', url: '' ]) {

                    sh "docker push nikitazhovnirenko/prikm:latest"

                    sh "docker push nikitazhovnirenko/prikm:\${2}"

                }

            }

        }

        stage('Deploy image'){

            steps {

                sh '''

                    docker stop my-web || true

                    docker rm my-web || true

                    docker run -d -p 80:80 --name my-web nikitazhovnirenko/prikm:latest

                '''

                echo '#  Container deployed successfully'

            }

        }

    }

}

