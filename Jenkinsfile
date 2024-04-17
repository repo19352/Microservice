pipeline {
    agent any
    environment {
         SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
         stage('sonar') {
            steps {
              withSonarQubeEnv('sonar-server') {
                sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=recommendationservice \
                    -Dsonar.projectKey=recommendationservice '''
                   }
            }
        }
         stage('docker-build') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t recommendationservice:latest ."
                       sh  "docker tag shippingservice:latest meena835/recommendationservice:latest"
                  }
               }
            }
        }
        stage('trivy') {
            steps {
              sh  "trivy image --format json -o report.json meena835/recommendationservice:latest"
            }
        }
         stage('docker-push') {
             steps {
               script {
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh  "docker push  meena835/recommendationservice:latest"
                  }
               }
            }
        }
    }
}

