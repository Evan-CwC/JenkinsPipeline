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
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests...'
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis...'
                sh 'mvn sonar:sonar'
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
                sh 'docker build -t my-app .'
sh 'docker run -d --name my-app-container -p 8080:8080 my-app'

            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging...'
                // Replace with actual integration test command
                sh 'ssh user@staging-server "java -jar /path/to/deploy/my-app.jar test"'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
                // Replace with actual deployment command
                sh 'scp target/my-app.jar user@production-server:/path/to/deploy'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "Good news, everyone! The build was successful.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nBuild URL: ${env.BUILD_URL}",
                to: "your-email@example.com"
            )
        }
        failure {
            emailext (
                subject: "FAILURE: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "Unfortunately, the build failed. Please check the logs for details.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nBuild URL: ${env.BUILD_URL}",
                to: "your-email@example.com"
            )
        }
        always {
            emailext (
                subject: "BUILD COMPLETE: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "The build has completed.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nBuild URL: ${env.BUILD_URL}",
                to: "your-email@example.com",
                attachLog: true
            )
        }
    }
}
