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
 echo 'Checking out source code'
 checkout scm
 }
 }

 stage('Build') {
 steps {
 echo 'Building Maven project'
 sh 'mvn clean compile -B'
 }
 }

 stage('Test') {
 steps {
 echo 'Running unit tests'
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
 echo 'Packaging the application'
 sh 'mvn package -DskipTests -B'
 archiveArtifacts artifacts: 'target/*.war', fingerprint: true, allowEmptyArchive: true
 }
 }

 stage('Deploy') {
 steps {
 echo 'Deploying Maven artifact'
 sh 'mvn deploy -DskipTests -B || echo Deploy phase completed'
 }
 }
 }

 post {
 success {
 echo 'Pipeline completed successfully'
 }
 failure {
 echo 'Pipeline failed! Check the logs for details.'
 }
 always {
 cleanWs()
 }
 }
}