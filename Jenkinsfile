
pipeline {
    agent any

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
                bat 'docker build -t student-flask-app .'
            }
        }

        stage('Stop Old Container') {
            steps {
                powershell '''
                    docker stop student-app
                    if ($LASTEXITCODE -ne 0) {
                        Write-Host "No existing container to stop."
                    }
                    docker rm student-app
                    if ($LASTEXITCODE -ne 0) {
                        Write-Host "No existing container to remove."
                    }
                    exit 0
                '''
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
