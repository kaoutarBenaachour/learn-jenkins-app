pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube') // Jenkins credential ID for SonarQube token
    }

    stages {
        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:4.8'
                    reuseNode true
                }
            }
            steps {
                script {
                    // Run SonarQube analysis
                    sh """
                    sonar-scanner \
                        -Dsonar.projectKey=jenkins_project \
                        -Dsonar.projectName=jenkins_project \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://127.0.0.1:9002 \
                        -Dsonar.login=$SONARQUBE_TOKEN
                    """
                }
            }
        }
    }
}
