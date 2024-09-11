pipeline {
    agent any
    
    environment {
        GIT_CREDENTIALS_ID = 'your-git-credentials-id' // Replace with your Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                checkout([$class: 'GitSCM', 
                          userRemoteConfigs: [[url: 'https://github.com/omnagare9975/Java-app.git', credentialsId: "${env.GIT_CREDENTIALS_ID}"]], 
                          branches: [[name: '*/main']]])
            }
        }
        
        stage('Build') {
            steps {
                // Using Maven from the environment, without specifying a version
                sh 'mvn clean install'
            }
        }

        stage('OWASP Dependency-Check') {
            steps {
                // OWASP Dependency-Check steps
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
        }

        stage('Archive Reports') {
            steps {
                // Archive reports
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true,
                             reportDir: 'target/site', reportFiles: 'index.html', reportName: 'HTML Report'])
            }
        }
    }
    
    post {
        always {
            // Cleanup actions if necessary
            echo 'Pipeline completed'
        }
        failure {
            // Actions to take on failure
            echo 'Build failed'
        }
    }
}
