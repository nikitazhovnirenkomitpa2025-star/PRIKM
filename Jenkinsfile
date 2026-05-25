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
        stage('Create Artifact') {
            steps {
                sh '''
                    echo "Build number: ${BUILD_NUMBER}" > build_info.txt
                    echo "Build date: $(date)" >> build_info.txt
                    echo "Docker image: nikitazhovnirenko/prikm:${BUILD_NUMBER}" >> build_info.txt
                '''
                archiveArtifacts artifacts: 'build_info.txt', fingerprint: true
            }
        }
        stage('Image Build') {
            steps {
                sh "docker build -t nikitazhovnirenko/prikm:${BUILD_NUMBER} ."
                sh "docker tag nikitazhovnirenko/prikm:${BUILD_NUMBER} nikitazhovnirenko/prikm:latest"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '3824467', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push nikitazhovnirenko/prikm:${BUILD_NUMBER}"
                    sh 'docker push nikitazhovnirenko/prikm:latest'
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
                echo "#  Application successfully deployed!"
            }
        }
    }
    post {
        always {
            echo '=== Pipeline finished ==='
        }
        success {
            echo '#  Pipeline completed SUCCESSFULLY'
        }
        failure {
            echo '#  Pipeline FAILED'
        }
    }
}
