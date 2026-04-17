pipeline {
    agent any

    environment {
        IMAGE_NAME = "student-app"
        CONTAINER_NAME = "student-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh '''
                docker stop student-app || true
                docker rm student-app || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t student-app .
                '''
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                docker run -d -p 5000:5000 \
                --name student-app \
                -e MONGODB_URI="mongodb://mongodb:27017/student_db" \
                student-app
                '''
            }
        }
    }

    post {
        success {
            echo "Build and Deployment Successful 🚀"
        }
        failure {
            echo "Build Failed ❌ Check logs"
        }
    }
}
