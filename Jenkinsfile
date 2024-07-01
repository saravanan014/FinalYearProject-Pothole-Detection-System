pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from version control
                git url: 'https://github.com/saravanan014/FinalYearProject-Pothole-Detection-System'
            }
        }

        stage('Gitleaks Scan') {
            steps {
                // Run Gitleaks to detect leaks
                sh 'gitleaks detect --source . -v'
            }
        }
    }

    post {
        success {
            // Example: Notify on success
            echo 'Pipeline succeeded! Gitleaks did not find any leaks.'
        }
        failure {
            // Example: Notify on failure
            echo 'Pipeline failed! Gitleaks detected potential leaks.'
        }
    }
}
