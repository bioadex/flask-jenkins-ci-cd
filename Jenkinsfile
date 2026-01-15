pipeline {
    agent any

    environment {
        APP_ENV = "production"
        IMAGE_NAME = "flask-jenkins-app"
        CONTAINER_NAME = "flask-prod"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bioadex/how-automate-with-jenkins.git'
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

        stage('Deploy to Production') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                  --name $CONTAINER_NAME \
                  -e APP_ENV=$APP_ENV \
                  -p 5000:5000 \
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
