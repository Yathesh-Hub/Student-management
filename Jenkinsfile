pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                # Update system
                sudo yum update -y

                # Install Docker
                sudo yum install -y docker
                sudo systemctl start docker
                sudo systemctl enable docker
                sudo usermod -aG docker ec2-user

                # Install Node.js
                curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
                sudo yum install -y nodejs

                # Verify installs
                docker --version
                node -v
                npm -v
                '''
            }
        }

        stage('Run MongoDB Container') {
            steps {
                sh '''
                # Remove old Mongo container if exists
                docker stop mongodb || true
                docker rm mongodb || true

                # Run MongoDB
                docker run -d -p 27017:27017 --name mongodb mongo:6
                '''
            }
        }

        stage('Install App Dependencies') {
            steps {
                sh '''
                npm install
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

        stage('Run App Container') {
            steps {
                sh '''
                docker stop student-app || true
                docker rm student-app || true

                docker run -d -p 5000:5000 \
                -e MONGODB_URI="mongodb://172.17.0.1:27017/student_db" \
                --name student-app student-app
                '''
            }
        }
    }
}
