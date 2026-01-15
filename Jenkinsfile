pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-jenkins-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bioadex/flask-jenkins-ci-cd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Deploy to Dev') {
            environment {
                APP_ENV = "dev"
                CONTAINER_NAME = "flask-dev"
                APP_PORT = "5001"
            }
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                  --name $CONTAINER_NAME \
                  -e APP_ENV=$APP_ENV \
                  -p $APP_PORT:5000 \
                  $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy to Staging') {
            environment {
                APP_ENV = "staging"
                CONTAINER_NAME = "flask-staging"
                APP_PORT = "5002"
            }
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                  --name $CONTAINER_NAME \
                  -e APP_ENV=$APP_ENV \
                  -p $APP_PORT:5000 \
                  $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Approve Production Deployment') {
            steps {
                input message: 'Deploy to Production?', ok: 'Deploy'
            }
        }

        stage('Deploy to Production') {
            environment {
                APP_ENV = "production"
                CONTAINER_NAME = "flask-prod"
                APP_PORT = "5000"
            }
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                  --name $CONTAINER_NAME \
                  -e APP_ENV=$APP_ENV \
                  -p $APP_PORT:5000 \
                  $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        success {
            mail to: 'adewalekomolafe@gmail.com',
                 subject: "SUCCESS: Jenkins Build #${BUILD_NUMBER}",
                 body: "The Flask app was successfully deployed."
        }
        failure {
            mail to: 'adewalekomolafe@gmail.com',
                 subject: "FAILED: Jenkins Build #${BUILD_NUMBER}",
                 body: "The Jenkins pipeline failed. Check the logs."
        }
    }
}

