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
                sh 'docker build -t prikm:latest .'
                sh 'docker tag prikm nikitazhovnirenko/prikm:latest'
            }
        }
        stage('Deploy image'){
            steps {
                sh '''
                    docker stop my-web || true
                    docker rm my-web || true
                    docker run -d -p 80:80 --name my-web nikitazhovnirenko/prikm:latest
                '''
                echo '#  Container deployed successfully on port 80'
            }
        }
    }
}
