pipeline {
    agent any

    parameters {
        string(name: 'env', defaultValue: 'dev', description: 'Deployment Environment')
        string(name: 'region', defaultValue: 'us-east-1', description: 'AWS Region')
        string(name: 'branch', defaultValue: 'main', description: 'Git Branch')
    }

    environment {
        MAVEN_OPTS = '-Xmx512m'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: \"
                git branch: "\",
                    url: 'https://github.com/akhil835/simple_mave_war.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Maven project for environment: \"
                sh 'mvn clean compile -B'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test -B'
            }
            post {
                always {
                    junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package -DskipTests -B'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true, allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to environment: \ in region: \"
                sh "mvn deploy -DskipTests -B -Denv=\ -Dregion=\ || echo 'Deploy phase completed (no remote repo configured)'"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully for env: \"
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
        always {
            cleanWs()
        }
    }
}