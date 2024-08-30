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
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    sh 'mvn dependency-check:check'
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
                    sh 'ssh user@staging-server "java -jar /path/to/deploy/my-app.jar test"'
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
        always {
            emailext body: "Pipeline completed. Check attached logs.",
                     recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                     subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName}",
                     to: "s219156788@deakin.edu.au",
                     attachLog: true
        }
    }
}
