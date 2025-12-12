pipeline {
    agent any  // Use a normal Jenkins node with Docker installed

    environment {
        IMAGE_NAME = "dockerhubusername/oruk-app:latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m pip install --upgrade pip
                pip3 install --no-cache-dir -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                export PYTHONPATH=$PYTHONPATH:$(pwd)/src
                pytest -q
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f oruk-app || true
                docker run -d --name oruk-app -p 5000:5000 ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
