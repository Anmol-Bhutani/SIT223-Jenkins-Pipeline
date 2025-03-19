pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        stage('Code Analysis') {
            steps {
                script {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=my-app'
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    sh 'zap-baseline.py -t http://your-staging-url'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                script {
                    sh 'scp target/my-app.jar user@staging-server:/path/to/deploy'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                script {
                    sh 'mvn verify -P staging'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    sh 'scp target/my-app.jar user@production-server:/path/to/deploy'
                }
            }
        }
    }

    post {
        success {
            emailext body: 'The pipeline succeeded!', subject: 'Pipeline Success', to: 'your-email@example.com'
        }
        failure {
            emailext body: 'The pipeline failed!', subject: 'Pipeline Failure', to: 'your-email@example.com'
        }
    }
}