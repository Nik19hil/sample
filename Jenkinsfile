pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsId: 'github-credentials', url: 'https://github.com/Nik19hil/todo-application.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                sh 'docker tag todo-application-image:latest docknik19/todo-application:latest'
                sh 'docker push docknik19/todo-application:latest'
            }
        }
        stage('Deploy with Docker Compose') {
            step {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }
        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
            }
        }
    }
    post {
        always {
            sh 'rm -rf *'
        }
    }
} 
