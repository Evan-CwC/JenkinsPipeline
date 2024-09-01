pipeline {
    agent any

    tools {
        maven 'Maven 3.9.9'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Running Tests...'
                sh 'mvn test'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Running Security Scan...'
                sh 'mvn dependency-check:check'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                sh 'scp target/my-app-1.0-SNAPSHOT.jar user@staging-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            emailext (
                subject: "BUILD COMPLETE: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "The build has completed.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nBuild URL: ${env.BUILD_URL}",
                to: "4933095@gmail.com",
                attachLog: true
            )
        }
    }
}
