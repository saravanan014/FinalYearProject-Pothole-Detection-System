/*pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from version control
                git branch:'main', url: 'https://github.com/saravanan014/FinalYearProject-Pothole-Detection-System.git'
            }
        }

        stage('Gitleaks Scan') {
            steps {
                // Run Gitleaks to detect leaks
                sh 'gitleaks detect --source . -v || true'
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

pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Scan') {
      steps {
        withSonarQubeEnv(installationName: 'sonar') { 
          sh './mvnw clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
        }
      }
    }
  }
} */

pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'sonar'  // Ensure this matches the name of your SonarQube server in Jenkins
        SONARQUBE_TOKEN = credentials('sonar')  // Replace with your SonarQube token ID from Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from version control
                git branch: 'main', url: 'https://github.com/saravanan014/FinalYearProject-Pothole-Detection-System.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    if (fileExists('./mvnw')) {
                        sh './mvnw clean install'
                    } else {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(installationName: "${SONARQUBE_SERVER}") {
                    script {
                        if (fileExists('./mvnw')) {
                            sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar -Dsonar.login=${SONARQUBE_TOKEN}'
                        } else {
                            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar -Dsonar.login=${SONARQUBE_TOKEN}'
                        }
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube analysis report and check quality gate status
                timeout(time: 1, unit: 'HOURS') {  // Adjust timeout if needed
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded! SonarQube analysis passed.'
        }
        failure {
            echo 'Pipeline failed! Please check the logs for details.'
        }
    }
}

