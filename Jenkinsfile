pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mariem507/cv-one-page'
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials') // ID des credentials Jenkins pour Docker Hub
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/mariammalki/cv-one-page.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo ${DOCKER_CREDENTIALS_PSW} | docker login -u ${DOCKER_CREDENTIALS_USR} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Notify Slack') {
            steps {
                slackSend(
                    channel: '#ci-cd',
                    tokenCredentialId: 'slack-bot-token',
                    message: "✅ Pipeline terminé avec succès : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                )
            }
        }
    }

    post {
        failure {
            slackSend(
                channel: '#ci-cd',
                tokenCredentialId: 'slack-bot-token',
                message: "❌ Échec du pipeline : ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
    }
}
