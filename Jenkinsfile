pipeline {
  agent any
  environment {
    PATH = "/opt/homebrew/opt/node@18/bin:${env.PATH}"
  }
  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test || true'
      }
    }
    stage('Security Scan (npm audit)') {
      steps {
        sh 'npm audit || true'
      }
    }
  }
}
