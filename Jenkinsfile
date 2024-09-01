pipeline {
    agent any

    tools {
        maven 'Maven 3.9.9' // Ensure Maven 3.9.9 is installed and configured in Jenkins
    }

    stages {
        // Stage 1: Build
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean package'
            }
        }

        // Stage 2: Unit Tests
        stage('Unit Tests') {
            steps {
                echo 'Running Unit Tests...'
                sh 'mvn test'
            }
        }

        // Stage 3: Integration Tests
        stage('Integration Tests') {
            steps {
                echo 'Running Integration Tests...'
                sh 'mvn verify'
            }
        }

        // Stage 4: Code Analysis (Skipping SonarQube as it's not available)
        stage('Static Code Analysis') {
            steps {
                echo 'Static Code Analysis...'
                // Include your preferred code analysis tool here
            }
        }

        // Stage 5: Security Scan
        stage('Security Scan') {
            steps {
                echo 'Running Security Scan...'
                sh 'mvn dependency-check:check'
                archiveArtifacts artifacts: 'target/dependency-check-report.html', fingerprint: true
            }
        }

        // Stage 6: Deploy to Staging
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                sh '''
                    scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar user@staging-server:/path/to/deploy/
                '''
            }
        }

        // Stage 7: Deploy to Production
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
                sh '''
                    scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar user@production-server:/path/to/deploy/
                '''
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "The build was successful.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nCheck details at: ${env.BUILD_URL}",
                to: "4933095@gmail.com",
                attachLog: true
            )
        }
        failure {
            emailext (
                subject: "FAILURE: Job '${env.JOB_NAME}' (${env.BUILD_NUMBER})",
                body: "The build failed.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nCheck details at: ${env.BUILD_URL}",
                to: "4933095@gmail.com",
                attachLog: true
            )
        }
    }
}
