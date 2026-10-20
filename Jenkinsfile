pipeline {
    agent any

    environment {
        CONTAINER_NAME = 'nestjs-app'
        IMAGE_NAME     = 'nestjs-image'
        EMAIL          = 'mdyusufak47@gmail.com'
        PORT           = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/iamyusuf01/CI-CD-Pipeline-Using-Jenkins-GitHub-Webhook-Ubuntu-EC2-Docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t "$IMAGE_NAME" .'
            }
        }

        stage('Stop and Remove Previous Container') {
            steps {
                sh '''
                    docker stop "$CONTAINER_NAME" || true
                    docker rm "$CONTAINER_NAME" || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker run -d \
                      --restart unless-stopped \
                      -p "$PORT:$PORT" \
                      --name "$CONTAINER_NAME" \
                      "$IMAGE_NAME"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps --filter "name=$CONTAINER_NAME"'
            }
        }

        stage('Send Email Notification') {
            steps {
                emailext(
                    subject: 'NestJS App Deployment Successful',
                    body: "Your NestJS application is deployed at http://13.61.32.229:${PORT}/",
                    to: "${EMAIL}"
                )
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Check the Jenkins Console Output.'
        }
        success {
            echo 'NestJS application deployed successfully!'
        }
    }
}