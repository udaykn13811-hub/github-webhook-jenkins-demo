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
                echo "======================================"
                echo " GitHub Webhook Triggered Jenkins"
                echo "======================================"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Pipeline started successfully"
            }
        }

        stage('Checkout Source Code') {
            steps {
                checkout scm

                script {
                    def branch = sh(
                        script: 'git branch --show-current',
                        returnStdout: true
                    ).trim()

                    echo "Checked out branch: ${branch}"
                }
            }
        }

        stage('Verify Tools') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'git --version'
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
            echo "GitHub webhook pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed. Check the console output"
        }

        always {
            echo "Pipeline execution completed"
        }
    }
}
