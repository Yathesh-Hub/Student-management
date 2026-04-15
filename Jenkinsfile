pipeline {
    agent any

    stage('Clone') {
    steps {
        git branch: 'main', url: 'https://github.com/Yathesh-Hub/Student-management.git'
        }
    }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop student-app || true
                docker rm student-app || true
                docker run -d -p 3000:3000 \
                -e MONGODB_URI="mongodb://host.docker.internal:27017/student_db" \
                --name student-app student-app
                '''
            }
        }
    }
}
