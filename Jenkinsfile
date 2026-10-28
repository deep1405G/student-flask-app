
pipeline {
    agent any

    environment {
    DOCKER_HOME = 'C:\\Users\\deepg\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin'
    PATH = "${env.PATH};${DOCKER_HOME}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Test Application') {
            steps {
                bat 'python -m py_compile app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
              bat '"C:\\Users\\deepg\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t student-flask-app .'
            }
        }

        stage('Stop Old Container') {
            steps {
                bat 'docker rm -f student-app 2>NUL || ver >NUL'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'docker run -d -p 5000:5000 --name student-app student-flask-app'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully using Jenkins.'
        }
        failure {
            echo 'Application deployment failed.'
        }
    }
}
