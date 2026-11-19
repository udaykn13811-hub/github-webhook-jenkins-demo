pipeline {

    agent any

    tools {
        jdk 'Java21'
        maven 'Maven3'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Webhook Trigger Validation') {
            steps {
                echo "Pipeline triggered automatically by GitHub"
                echo "Build Number: ${BUILD_NUMBER}"
                echo "Git Branch: ${BRANCH_NAME}"
            }
        }

        stage('Checkout Source Code') {
            steps {
                checkout scm
                echo "Source code downloaded successfully"
            }
        }

        stage('Verify Tools') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
            }
        }

        stage('Compile Application') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package Application') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {

        success {
            echo "Webhook triggered pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed. Check Jenkins console output."
        }

        always {
            echo "Pipeline execution completed"
        }
    }
}
