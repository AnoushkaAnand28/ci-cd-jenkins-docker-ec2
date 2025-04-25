pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
        DOCKER_IMAGE = 'anoushkaanand28/ci-cd-demo'
        SSH_KEY = credentials('ec2-ssh-key-id')
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/AnoushkaAnand28/ci-cd-jenkins-docker-ec2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred-id') {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['ec2-ssh-key-id']) {
                    sh '''
                        # Automatically add the EC2 host's SSH key to known_hosts
                        ssh-keyscan -H 13.49.57.179 >> ~/.ssh/known_hosts

                        # Proceed with the deployment
                        ssh ec2-user@13.49.57.179 << EOF
                        set -e
                        docker pull anoushkaanand28/ci-cd-demo:latest
                        docker stop demo || true
                        docker rm demo || true
                        docker run -d --name demo -p 5000:3000 anoushkaanand28/ci-cd-demo:latest
                        EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build or deployment failed. Check the logs.'
        }
    }
}
