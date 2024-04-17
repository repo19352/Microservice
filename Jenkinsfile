pipeline {
    agent any
    environment {
         SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
         stage('sonar') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=frontend \
                    -Dsonar.projectKey=frontend '''
                   }
            }
        }
         stage('docker-build') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t frontend:latest ."
                       sh  "docker tag frontend:latest meena835/frontend:latest"
                  }
               }
            }
        }
        stage('trivy') {
            steps {
              sh  "trivy image --format json -o report.json meena835/frontend:latest"
            }
        }
         stage('docker-push') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh  "docker push  meena835/frontend:latest"
                  }
               }
            }
        }
    }
}
