pipeline {
    agent any

    parameters {
        string(name: 'env', defaultValue: 'dev', description: 'Deployment environment')
        string(name: 'region', defaultValue: 'us-east-1', description: 'AWS region')
        string(name: 'branch', defaultValue: 'main', description: 'Git branch to build')
    }

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        APP_NAME = 'simple_mave_war'
        DEPLOY_ENV = "${params.env}"
        AWS_REGION = "${params.region}"
        BUILD_BRANCH = "${params.branch}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${BUILD_BRANCH}"
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${BUILD_BRANCH}"]],
                    userRemoteConfigs: [[url: 'https://github.com/akhil835/simple_mave_war.git']]
                ])
            }
        }

        stage('Build Info') {
            steps {
                echo "Building: ${APP_NAME}"
                echo "Environment: ${DEPLOY_ENV}"
                echo "Region: ${AWS_REGION}"
                echo "Branch: ${BUILD_BRANCH}"
                sh 'mvn --version'
                sh 'java -version'
            }
        }

        stage('Maven Clean') {
            steps {
                echo 'Running Maven clean...'
                sh 'mvn clean'
            }
        }

        stage('Maven Compile') {
            steps {
                echo 'Compiling source code...'
                sh 'mvn compile'
            }
        }

        stage('Maven Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Maven Package') {
            steps {
                echo 'Packaging application into WAR...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving WAR artifacts...'
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true, allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            when {
                expression { params.env == 'dev' || params.env == 'prod' }
            }
            steps {
                echo "Deploying to environment: ${DEPLOY_ENV} in region: ${AWS_REGION}"
                echo 'Deployment step - configure as needed for your target environment'
            }
        }
    }

    post {
        success {
            echo "Build SUCCESS: ${APP_NAME} built and packaged successfully!"
            echo "Artifact: target/*.war is ready for deployment."
        }
        failure {
            echo "Build FAILED: Check the logs for errors."
        }
        always {
            echo "Build completed with status: ${currentBuild.result}"
            cleanWs()
        }
    }
}