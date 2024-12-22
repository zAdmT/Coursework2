pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'GitHubCredentials', url: 'git@github.com:<your-username>/<repo-name>.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t <your-dockerhub-username>/cw2-server:latest .'
            }
        }
        stage('Test Container') {
            steps {
                sh 'docker run -d -p 8080:8080 <your-dockerhub-username>/cw2-server:latest'
                sh 'curl -f http://localhost:8080 || exit 1'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker login -u <your-dockerhub-username> -p <your-dockerhub-password>'
                sh 'docker push <your-dockerhub-username>/cw2-server:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/cw2-server cw2-server=<your-dockerhub-username>/cw2-server:latest'
            }
        }
    }
}
