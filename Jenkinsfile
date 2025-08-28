pipeline {
    agent any

    tools {
        maven 'Maven 3.9.11'
    }

    options {
        // Keep only last 5 builds (logs + artifacts + reports)
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

        stage('Archive Results') {
            steps {
                // Archive both .jtl files and the full HTML dashboard
                archiveArtifacts artifacts: 'target/jmeter/results/**/*', allowEmptyArchive: false
            }
        }

        stage('Publish JMeter HTML Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target/jmeter/results',
                    reportFiles: 'index.html',
                    reportName: 'JMeter HTML Report'
                ])
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
