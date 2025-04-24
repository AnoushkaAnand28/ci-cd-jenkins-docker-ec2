pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('anoushkaanand28')
        DOCKER_IMAGE = 'anoushkaanand28/ci-cd-demo'
        SSH_KEY = credentials('ec2-ssh-key-id')
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/AnoushkaAnand28/ci-cd-jenkins-docker-ec2.git'
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
                        ssh -o StrictHostKeyChecking=no ec2-user@EC2_PUBLIC_IP '
                        docker pull anoushkaanand28/ci-cd-demo:latest &&
                        docker stop demo || true &&
                        docker rm demo || true &&
                        docker run -d --name demo -p 80:3000 anoushkaanand28/ci-cd-demo:latest
                        '
                    '''
                }
            }
        }
    }
}
