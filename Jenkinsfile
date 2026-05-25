pipeline {
    agent any
    
    parameters {
        string(name: 'MESSAGE', defaultValue: 'Hello from Jenkins!', description: 'Message for Telegram')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'test', 'prod'], description: 'Environment')
    }
    
    triggers {
        cron('H/5 * * * *')
    }
    
    stages {
        stage('Start') {
            steps {
                echo "=== Lab_3: Build #${BUILD_NUMBER} started ==="
                echo "Environment: ${params.ENVIRONMENT}"
            }
        }
        
        stage('Build Docker Image') {
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
            }
        }
    }
    
    post {
        success {
            sh """
                curl -s -X POST https://api.telegram.org/bot8536779258:AAE-EsV_vkxHYF1bClFconkVp0YQyQobI5U/sendMessage \
                -d chat_id=399053035 \
                -d text="#  SUCCESS! Build #${BUILD_NUMBER} completed successfully.\\nEnvironment: ${params.ENVIRONMENT}\\n${params.MESSAGE}"
            """
        }
        failure {
            sh """
                curl -s -X POST https://api.telegram.org/bot8536779258:AAE-EsV_vkxHYF1bClFconkVp0YQyQobI5U/sendMessage \
                -d chat_id=399053035 \
                -d text="#  FAILED! Build #${BUILD_NUMBER} has failed."
            """
        }
        always {
            echo '=== Lab_3 finished ==='
        }
    }
}
