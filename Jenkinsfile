pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK_HOME = '/var/jenkins_home/dependency-check'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout from the correct repository URL
                checkout([$class: 'GitSCM', 
                          userRemoteConfigs: [[url: 'https://github.com/omnagare9975/Java-app.git', credentialsId: "${env.GIT_CREDENTIALS_ID}"]], 
                          branches: [[name: '*/main']]]) // Ensure the branch name is correct
            }
        }

        stage('Build') {
            steps {
                // Use Maven tool specified in Jenkins global configuration
                tool name: 'Maven 3.8.6', type: 'maven'
                sh 'mvn clean install'
            }
        }

        stage('OWASP Dependency-Check') {
            steps {
                dependencyCheck odcInstallation: 'Default',
                                additionalArguments: '--format HTML --format XML --outdir dependency-check-report --scan .'
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: 'dependency-check-report/**/*'
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'dependency-check-report',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'OWASP Dependency-Check Report'
                ]
            }
        }
    }
}
