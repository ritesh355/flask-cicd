pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ritesh355/flask-app:${BUILD_NUMBER}"
        DEPLOY_SERVER = 'ubuntu@13.201.139.141'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ritesh355/flask-cicd.git', branch: 'main', credentialsId: 'github-pat'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build DOCKER_IMAGE
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_SERVER} '
                            docker pull ${DOCKER_IMAGE}
                            docker stop flask-container || true
                            docker rm flask-container || true
                            docker run -d --name flask-container -p 5000:5000 ${DOCKER_IMAGE}
                        '
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
