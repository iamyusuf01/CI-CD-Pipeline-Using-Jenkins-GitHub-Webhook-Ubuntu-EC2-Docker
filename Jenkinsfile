pipeline {
    agent any

    environment {
        CONTAINER_NAME = 'nestjs-app'
        IMAGE_NAME = 'nestjs-image'
        EMAIL = 'mdyusufak47@gamil.com'
        PORT = '3000'
    }

    stages {
        stage('Clone Repo') {
            step {
                git(
                    branch: 'main',
                    url: 'https://github.com/iamyusuf01/CI-CD-Pipeline-Using-Jenkins-GitHub-Webhook-Ubuntu-EC2-Docker'
                )
            }
        }
        stage('Build Docker Image'){
            steps {
                sh 'docker build -t $IMAGE_NAME'
            }
        }
        stage('Stop & Remove Previous Container'){
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                
                '''
            }
        }
        stage('Docker Container Run'){
            steps {
                sh '''
                    docker run -d -p ${PORT}:${PORT}
                    --name $CONTAINER_NAME $IMAGE_NAME
                
                '''
            }
        }
        stage('Send Email Notification'){
            steps {
                emailtext(
                    subject:'Nest Js App Deployed Successfully on EC2!'
                    body: 'Your Nest JS app is Deployed!
                    http://13.61.32.229:${PORT}/'
                    to: '${EMAIL}'
                )
            }
        }
    }
}