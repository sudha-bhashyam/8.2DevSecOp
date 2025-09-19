pipeline {
  agent any
  environment {
    PATH = "/opt/homebrew/opt/node@18/bin:${env.PATH}"
    RECIPIENTS = "lakshmisudha909@gmail.com"
    FROM_ADDR = "lakshmisudha909@gmail.com"
  }

  stages {
    stage('Install Dependencies') {
      steps { sh 'npm install' }
    }

    stage('Run Tests') {
      steps {
        sh 'set +e; npm test 2>&1 | tee test.log; true'
        sh 'ls -l test.log || echo "no test.log"'
      }
      post {
        always {
          emailext(
            from: env.FROM_ADDR,
            to: env.RECIPIENTS,
            subject: "Run Tests - ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
            body: "Run Tests finished with ${currentBuild.currentResult}. See attached log.",
            attachmentsPattern: "test.log",
            mimeType: 'text/plain'
          )
        }
      }
    }

    stage('Security Scan (npm audit)') {
      steps {
        sh 'set +e; npm audit 2>&1 | tee audit.log; true'
        sh 'ls -l audit.log || echo "no audit.log"'
      }
      post {
        always {
          emailext(
            from: env.FROM_ADDR,
            to: env.RECIPIENTS,
            subject: "Security Scan - ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
            body: "Security Scan finished with ${currentBuild.currentResult}. See attached log.",
            attachmentsPattern: "audit.log",
            mimeType: 'text/plain'
          )
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'test.log,audit.log', allowEmptyArchive: true
    }
  }
}
