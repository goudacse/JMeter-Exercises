pipeline {
    agent any

    tools {
        maven 'Maven 3.9.11'
    }

    options {
        // Keep only last 5 builds (logs + artifacts + reports)
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
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
                // Archive JTL files and generated HTML reports
                archiveArtifacts artifacts: 'target/jmeter/results/**/*', allowEmptyArchive: true
                archiveArtifacts artifacts: 'target/jmeter/reports/**/*', allowEmptyArchive: true
            }
        }

        stage('Publish JMeter HTML Report') {
            steps {
                script {
                    // Detect latest results folder under target/jmeter/results
                    def latestFolder = bat(
                        script: 'dir /b /ad /o-d "target\\jmeter\\results" > latest.txt && for /F %%i in (latest.txt) do @echo %%i',
                        returnStdout: true
                    ).trim()

                    def reportDir = "target/jmeter/results/${latestFolder}"

                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: reportDir,
                        reportFiles: 'index.html',
                        reportName: 'JMeter HTML Report'
                    ])
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
