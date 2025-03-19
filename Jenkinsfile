pipeline {
    agent any

    tools {
        maven 'Jenkinsfile' // This must match the name configured in Jenkins
    }

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
            emailext (
                subject: 'Pipeline Success: ${JOB_NAME} - Build #${BUILD_NUMBER}',
                body: 'The pipeline succeeded! Check the build details at: ${BUILD_URL}',
                to: 'anmol4762.be23@chitkara.edu.in' 
            )
        }
        failure {
            emailext (
                subject: 'Pipeline Failed: ${JOB_NAME} - Build #${BUILD_NUMBER}',
                body: 'The pipeline failed! Check the build details at: ${BUILD_URL}',
                to: 'anmol4762.be23@chitkara.edu.in' 
            )
        }
    }
}