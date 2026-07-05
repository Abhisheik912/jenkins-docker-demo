pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-docker-demo"
        CONTAINER_NAME = "jenkins-docker-demo-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Abhisheik912/jenkins-docker-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Docker Run') {
            steps {
                bat '''
                docker rm -f %CONTAINER_NAME% 2>nul || exit 0
                docker run -d --name %CONTAINER_NAME% -p 3000:3000 %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed. App should be running on http://localhost:3000'
        }
        failure {
            echo 'Pipeline failed. Check logs above.'
        }
    }
}
