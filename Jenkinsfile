pipeline {
  agent any
  environment {
    PATH = "/opt/homebrew/opt/node@18/bin:${env.PATH}"
    RECIPIENTS = "lakshmisudha909@gmail.com"
  }

  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'set +e; npm test 2>&1 | tee test.log; true'
      }
      post {
        always {
          emailext(
            to: env.RECIPIENTS,
            subject: "Run Tests - ${currentBuild.currentResult}",
            body: "Run Tests stage finished with ${currentBuild.currentResult}. See attached log.",
            attachmentsPattern: "test.log"
          )
        }
      }
    }

    stage('Security Scan (npm audit)') {
      steps {
        sh 'set +e; npm audit 2>&1 | tee audit.log; true'
      }
      post {
        always {
          emailext(
            to: env.RECIPIENTS,
            subject: "Security Scan - ${currentBuild.currentResult}",
            body: "Security Scan stage finished with ${currentBuild.currentResult}. See attached log.",
            attachmentsPattern: "audit.log"
          )
        }
      }
    }
  }
}
