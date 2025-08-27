pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/goudacse/JMeter-Exercises.git'
      }
    }

    stage('Run JMeter via Maven') {
      steps {
        bat "mvn clean verify"
      }
    }

    stage('Publish Artifacts & Reports') {
      steps {
        archiveArtifacts artifacts: 'target/jmeter/results/*.jtl', fingerprint: true
        publishHTML([
          allowMissing: false,
          alwaysLinkToLastBuild: true,
          keepAll: true,
          reportDir: 'target/jmeter/reports',
          reportFiles: 'index.html',
          reportName: 'JMeter HTML Report'
        ])
      }
    }
  }

  post {
    always { cleanWs() }
  }
}
