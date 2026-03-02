pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build + Test + JaCoCo') {
      steps {
        bat 'mvn -B clean test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          archiveArtifacts artifacts: 'target/site/jacoco/**', fingerprint: true
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        bat 'docker build -t temperature-converter:local .'
      }
    }

    stage('Run Docker Container') {
      steps {
        bat 'docker run --rm temperature-converter:local'
      }
    }
  }
}