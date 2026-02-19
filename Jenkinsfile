pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // npm ci will install exact versions from package-lock.json
                sh 'npm ci'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests, but do not fail pipeline if tests fail
                sh 'npm test || true'
            }
            post {
                always {
                    // Collect test results if any junit XML files exist
                    junit '**/junit*.xml'
                }
            }
        }

        stage('Archive Build') {
            steps {
                // Archive the built dist folder
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build Success'
        }
        failure {
            echo 'Build Failed'
        }
    }
}
